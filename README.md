Private Sub Worksheet_Change(ByVal Target As Range)
    On Error GoTo SafeExit

    ' Only act when C2 changes
    If Intersect(Target, Me.Range("C2")) Is Nothing Then Exit Sub

    Application.EnableEvents = False

    Dim countRows As Long
    countRows = 0
    If IsNumeric(Me.Range("C2").Value) Then
        countRows = CLng(Me.Range("C2").Value)
    End If

    ' If C2 is blank or <= 0, clear C8 and exit
    If countRows <= 0 Then
        Me.Range("C8").ClearContents
        GoTo SafeExit
    End If

    ' Find first non-empty cell in column N (from row 4 downward)
    Dim firstRow As Long
    firstRow = 0

    ' If everything is empty below N4, End(xlDown) can jump to bottom,
    ' so loop safely until a non-empty is found or reach last row.
    Dim r As Long, lastR As Long
    lastR = Me.Cells(Me.Rows.Count, "N").End(xlUp).Row
    If lastR < 4 Then lastR = Me.Rows.Count

    For r = 4 To lastR
        If Len(Me.Cells(r, "N").Value) > 0 Then
            firstRow = r
            Exit For
        End If
    Next r

    ' If none found, clear C8 and exit
    If firstRow = 0 Then
        Me.Range("C8").ClearContents
        GoTo SafeExit
    End If

    ' Build the SUM range from firstRow to firstRow + countRows - 1
    Dim startAddr As String, endAddr As String
    startAddr = Me.Cells(firstRow, "N").Address(False, False)
    endAddr = Me.Cells(firstRow + countRows - 1, "N").Address(False, False)

    ' Write formula into C8
    Me.Range("C8").Formula = "=SUM(" & startAddr & ":" & endAddr & ")"

SafeExit:
    Application.EnableEvents = True
End Sub