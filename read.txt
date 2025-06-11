<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CQMT RANDOM SAMPLES</title>
    <!-- Bootstrap CSS for styling and layout -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
    <!-- Select2 CSS for searchable dropdowns -->
    <link href="https://cdn.jsdelivr.net/npm/select2@4.1.0-rc.0/dist/css/select2.min.css" rel="stylesheet">
    <!-- flatpickr CSS for date picker -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/flatpickr@4.6.13/dist/flatpickr.min.css">
    <style>
        /* General body styling with light yellow background */
        body {
            background-color: #FFF9E6;
            font-family: 'Inter', Arial, sans-serif;
            padding: 20px;
        }
        /* Card styling with 3D effect for inputs */
        .card-3d {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
            border-radius: 8px;
            padding: 20px;
            background: white;
            margin-bottom: 20px;
        }
        /* Hover effect for cards */
        .card-3d:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.25);
        }
        /* Glow animation for enabled cards */
        .card-enabled {
            animation: glow 1.5s ease-in-out infinite alternate;
        }
        /* Fade effect for disabled cards */
        .card-disabled {
            opacity: 0.5;
            pointer-events: none;
            animation: fadeDisable 0.5s ease;
        }
        /* Ensure Select2 dropdowns span full width */
        .select2-container {
            width: 100% !important;
        }
        /* Style Select2 inputs */
        .select2-container--default .select2-selection--single,
        .select2-container--default .select2-selection--multiple {
            border: 1px solid #ced4da;
            border-radius: 4px;
            padding: 8px;
            min-height: 38px;
        }
        .select2-selection__rendered {
            line-height: 22px !important;
        }
        /* Style date input */
        .date-input {
            white-space: nowrap;
            resize: none;
            min-height: 38px;
            line-height: 1.5;
            padding: 8px;
            border: 1px solid #ced4da;
            border-radius: 4px;
            width: 100%;
        }
        /* Multi-line date input for >3 selections */
        .date-input.multi-line {
            white-space: pre-wrap;
            height: auto;
        }
        /* Style buttons with 3D effect */
        .btn-clear, .btn-submit {
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
            padding: 10px 20px;
            border-radius: 4px;
        }
        .btn-clear:hover, .btn-submit:hover {
            transform: scale(1.1);
            box-shadow: 0 12px 25px rgba(0, 0, 0, 0.3);
        }
        .btn-clear:active, .btn-submit:active {
            transform: translateZ(-8px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
        }
        /* Style tooltips */
        .tooltip-inner {
            background: linear-gradient(45deg, #333, #555);
            color: white;
            font-size: 14px;
            padding: 10px;
            border-radius: 6px;
            opacity: 0.95;
        }
        .tooltip.bs-tooltip-top .tooltip-arrow::before {
            border-top-color: #333;
        }
        /* Animation keyframes */
        .fade-out {
            animation: fadeOut 0.5s ease forwards;
        }
        .fade-in {
            animation: fadeIn 0.5s ease forwards;
        }
        .explode {
            animation: explode 0.5s ease forwards;
        }
        .pulse {
            animation: pulse 0.6s ease infinite;
        }
        /* Progress overlay for submission */
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .progress-bar {
            width: 0;
            height: 6px;
            background: #4CAF50;
            border-radius: 3px;
            transition: width 0.1s ease;
        }
        .progress-text {
            color: white;
            font-size: 18px;
            margin-bottom: 10px;
        }
        /* Animation definitions */
        @keyframes fadeOut {
            from { opacity: 1; }
            to { opacity: 0; }
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        @keyframes explode {
            from { transform: scale(1) rotate(0deg); opacity: 1; }
            to { transform: scale(1.5) rotate(15deg); opacity: 0; }
        }
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.06); }
            100% { transform: scale(1); }
        }
        @keyframes glow {
            from { box-shadow: 0 0 5px #4CAF50; }
            to { box-shadow: 0 0 15px #4CAF50; }
        }
        @keyframes fadeDisable {
            from { opacity: 1; }
            to { opacity: 0.5; }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Page title -->
        <h1 class="text-center mb-4">CQMT RANDOM SAMPLES</h1>
        <div class="row">
            <!-- Rater Name input card -->
            <div class="col-md-4">
                <div class="card-3d card-enabled" id="rater-card" data-bs-toggle="tooltip" data-bs-placement="top" title="Select a rater">
                    <label for="rater-name" class="form-label">Rater Name</label>
                    <select id="rater-name" class="form-select" data-placeholder="Select Rater">
                        <option></option>
                    </select>
                </div>
            </div>
            <!-- Agent Name input card (disabled initially) -->
            <div class="col-md-4">
                <div class="card-3d card-disabled" id="agent-card" data-bs-toggle="tooltip" data-bs-placement="top" title="Select one or more Agents or select All for all Agents">
                    <label for="agent-name" class="form-label">Agent Name</label>
                    <select id="agent-name" class="form-select" multiple data-placeholder="Select Agents or All" disabled>
                        <option value="all">All</option>
                    </select>
                </div>
            </div>
            <!-- Load Date input card (disabled initially) -->
            <div class="col-md-4">
                <div class="card-3d card-disabled" id="date-card" data-bs-toggle="tooltip" data-bs-placement="top" title="Select one or more dates">
                    <label for="load-date" class="form-label">Load Date</label>
                    <input id="load-date" class="date-input" placeholder="Select Dates" disabled>
                </div>
            </div>
        </div>
        <!-- Buttons for Clear and Submit -->
        <div class="row mt-4">
            <div class="col-md-12 d-flex justify-content-between">
                <button id="clear-btn" class="btn btn-secondary btn-clear">Clear</button>
                <button id="submit-btn" class="btn btn-primary btn-submit">Submit</button>
            </div>
        </div>
        <!-- Progress overlay for submission -->
        <div id="overlay" class="overlay hidden">
            <div class="progress-text">Submitting... <span id="progress-percent">0%</span></div>
            <div id="progress-bar" class="progress-bar"></div>
        </div>
    </div>

    <!-- jQuery for DOM manipulation and AJAX -->
    <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <!-- Bootstrap JS for tooltips and utilities -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
    <!-- Select2 JS for searchable dropdowns -->
    <script src="https://cdn.jsdelivr.net/npm/select2@4.1.0-rc.0/dist/js/select2.min.js"></script>
    <!-- flatpickr JS for date picker -->
    <script src="https://cdn.jsdelivr.net/npm/flatpickr@4.6.13/dist/flatpickr.min.js"></script>
    <script>
        // Avoid jQuery conflicts
        const $ = jQuery.noConflict();

        $(document).ready(function () {
            console.log('DOM ready, script started');

            try {
                // Calculate dynamic date range (today to past 30 days)
                const today = new Date();
                const past30Days = Array.from({ length: 30 }, (_, i) => {
                    const date = new Date(today);
                    date.setDate(today.getDate() - i);
                    return date.toISOString().split('T')[0];
                }).reverse();

                // Initialize Select2 for Rater Name
                let $raterName = $('#rater-name');
                if ($raterName.length) {
                    $raterName.select2({
                        placeholder: 'Select Rater',
                        allowClear: true,
                        width: '100%'
                    });
                    console.log('Rater Name Select2 initialized');
                    // Fetch raters from backend
                    $.get('/get_raters', function (data) {
                        $raterName.empty().append('<option></option>');
                        data.forEach(rater => {
                            $raterName.append(new Option(rater, rater));
                        });
                    }).fail(function () {
                        console.error('Failed to fetch raters');
                    });
                } else {
                    console.error('Rater Name element not found');
                }

                // Initialize Select2 for Agent Name (disabled initially)
                let $agentName = $('#agent-name');
                if ($agentName.length) {
                    $agentName.select2({
                        placeholder: 'Select Agents or All',
                        allowClear: true,
                        width: '100%',
                        data: [{ id: 'all', text: 'All', selected: true }]
                    });
                    console.log('Agent Name Select2 initialized');
                } else {
                    console.error('Agent Name element not found');
                }

                // Initialize flatpickr for Load Date (disabled initially)
                let datePicker = null;
                let $loadDate = $('#load-date');
                if ($loadDate.length) {
                    datePicker = flatpickr('#load-date', {
                        mode: 'multiple',
                        dateFormat: 'Y-m-d',
                        maxDate: today,
                        minDate: past30Days[0],
                        enable: [],
                        disable: past30Days
                    });
                    console.log('flatpickr initialized');
                } else {
                    console.error('Load Date element not found');
                }

                // Handle multi-line dates for >3 selections
                $loadDate.on('change', function () {
                    try {
                        if (!$loadDate.length) {
                            console.error('Load Date element not found in date change handler');
                            return;
                        }
                        const selectedDates = $loadDate.val() ? $loadDate.val().split(', ') : [];
                        console.log('Dates changed:', selectedDates);
                        $loadDate.toggleClass('multi-line', selectedDates.length > 3);
                    } catch (e) {
                        console.error('Error in date multi-line handler:', e);
                    }
                });

                // Handle rater selection to enable/disable Agent Name
                $raterName.on('change', function () {
                    try {
                        if (!$raterName.length) {
                            console.error('Rater Name element not found in change handler');
                            return;
                        }
                        const selectedRater = $raterName.val();
                        console.log('Rater changed:', selectedRater);

                        // Animate and clear other inputs
                        if ($loadDate.length && $agentName.length) {
                            $loadDate.addClass('fade-out explode');
                            $agentName.addClass('fade-out explode');
                            setTimeout(() => {
                                if (datePicker && typeof datePicker.clear === 'function') {
                                    console.log('Clearing datePicker');
                                    datePicker.clear();
                                } else {
                                    console.warn('datePicker.clear is not available');
                                    $loadDate.val('');
                                }
                                $loadDate.removeClass('multi-line');
                                $agentName.val(null).trigger('change');
                                $agentName.select2({
                                    data: [{ id: 'all', text: 'All', selected: true }],
                                    width: '100%'
                                });
                                $loadDate.removeClass('fade-out explode').addClass('fade-in');
                                $agentName.removeClass('fade-out explode').addClass('fade-in');
                                setTimeout(() => {
                                    $loadDate.removeClass('fade-in');
                                    $agentName.removeClass('fade-in');
                                }, 500);

                                // Enable/disable Agent Name based on rater selection
                                if (selectedRater) {
                                    $agentName.prop('disabled', false);
                                    $('#agent-card').removeClass('card-disabled').addClass('card-enabled');
                                    // Fetch agents from backend
                                    $.post('/get_agents', { rater_name: selectedRater }, function (data) {
                                        $agentName.empty();
                                        data.forEach(agent => {
                                            $agentName.append(new Option(agent, agent, agent === 'all', agent === 'all'));
                                        });
                                        $agentName.trigger('change');
                                    }).fail(function () {
                                        console.error('Failed to fetch agents');
                                    });
                                } else {
                                    $agentName.prop('disabled', true);
                                    $('#agent-card').removeClass('card-enabled').addClass('card-disabled');
                                    $loadDate.prop('disabled', true);
                                    $('#date-card').removeClass('card-enabled').addClass('card-disabled');
                                }
                            }, 500);
                        }

                        // Reset date picker if no rater selected
                        if (!selectedRater && datePicker && typeof datePicker.set === 'function') {
                            datePicker.set('enable', []);
                            datePicker.set('disable', past30Days);
                        }
                    } catch (e) {
                        console.error('Error in rater change handler:', e);
                    }
                });

                // Handle agent selection to enable/disable Load Date and update dropdown
                $agentName.on('select2:select', function (e) {
                    try {
                        if (!$agentName.length) {
                            console.error('Agent Name element not found in select handler');
                            return;
                        }
                        let selected = $agentName.val() || [];
                        console.log('Agent selected:', selected);

                        // Remove 'all' if other agents are selected
                        if (selected.includes('all') && selected.length > 1) {
                            selected = selected.filter(val => val !== 'all');
                            $agentName.val(selected).trigger('change');
                        }

                        const selectedRater = $raterName.val();
                        // Enable Load Date if specific agents are selected
                        if ($loadDate.length && selectedRater && selected.length && !selected.includes('all')) {
                            $loadDate.prop('disabled', false);
                            $('#date-card').removeClass('card-disabled').addClass('card-enabled');
                            // Fetch dates for the first selected agent
                            $.post('/get_dates', {
                                rater_name: selectedRater,
                                agent_name: selected[0]
                            }, function (data) {
                                if (datePicker && typeof datePicker.set === 'function') {
                                    datePicker.set('enable', data);
                                    datePicker.set('disable', past30Days.filter(d => !data.includes(d)));
                                }
                            }).fail(function () {
                                console.error('Failed to fetch dates');
                            });
                        } else {
                            $loadDate.prop('disabled', true);
                            $('#date-card').removeClass('card-enabled').addClass('card-disabled');
                        }

                        // Update dropdown to hide selected agents
                        const availableAgents = [...new Set(
                            dummyData.filter(d => 
                                d.rater === selectedRater &&
                                ($loadDate.val() ? $loadDate.val().split(', ').includes(d.date) : true)
                            ).map(d => d.agent)
                        )].sort().filter(agent => !selected.includes(agent));
                        $agentName.select2({
                            data: [{ id: 'all', text: 'All', selected: selected.length === 0 }].concat(
                                availableAgents.map(agent => ({ id: agent, text: agent }))
                            ),
                            width: '100%'
                        });
                        $agentName.val(selected).trigger('change');
                    } catch (e) {
                        console.error('Error in agent select handler:', e);
                    }
                });

                // Handle agent deselection
                $agentName.on('select2:unselect', function () {
                    try {
                        if (!$agentName.length) {
                            console.error('Agent Name element not found in unselect handler');
                            return;
                        }
                        const selected = $agentName.val() || [];
                        console.log('Agent unselected:', selected);

                        const selectedRater = $raterName.val();
                        const availableAgents = [...new Set(
                            dummyData.filter(d => 
                                d.rater === selectedRater &&
                                ($loadDate.val() ? $loadDate.val().split(', ').includes(d.date) : true)
                            ).map(d => d.agent)
                        )].sort().filter(agent => !selected.includes(agent));
                        $agentName.select2({
                            data: [{ id: 'all', text: 'All', selected: selected.length === 0 }].concat(
                                availableAgents.map(agent => ({ id: agent, text: agent }))
                            ),
                            width: '100%'
                        });
                        $agentName.val(selected.length === 0 ? ['all'] : selected).trigger('change');

                        // Disable Load Date if no agents selected
                        if (!selected.length || selected.includes('all')) {
                            $loadDate.prop('disabled', true);
                            $('#date-card').removeClass('card-enabled').addClass('card-disabled');
                        }
                    } catch (e) {
                        console.error('Error in agent unselect handler:', e);
                    }
                });

                // Handle Clear button click
                $('#clear-btn').on('click', function () {
                    try {
                        console.log('Clear button clicked');
                        const $raterName = $('#rater-name');
                        const $loadDate = $('#load-date');
                        const $agentName = $('#agent-name');
                        if ($raterName.length && $loadDate.length && $agentName.length) {
                            // Apply explode animation
                            $raterName.addClass('fade-out explode');
                            $loadDate.addClass('fade-out explode');
                            $agentName.addClass('fade-out explode');
                            setTimeout(() => {
                                // Reset inputs
                                $raterName.val('').trigger('change');
                                if (datePicker && typeof datePicker.clear === 'function') {
                                    console.log('Clearing datePicker');
                                    datePicker.clear();
                                } else {
                                    console.warn('datePicker.clear is not available');
                                    $loadDate.val('');
                                }
                                $loadDate.removeClass('multi-line');
                                $agentName.val(null).trigger('change');
                                $agentName.select2({
                                    data: [{ id: 'all', text: 'All', selected: true }],
                                    width: '100%'
                                });
                                // Disable Agent Name and Load Date
                                $agentName.prop('disabled', true);
                                $loadDate.prop('disabled', true);
                                $('#agent-card').removeClass('card-enabled').addClass('card-disabled');
                                $('#date-card').removeClass('card-enabled').addClass('card-disabled');
                                // Reset animations
                                $raterName.removeClass('fade-out explode').addClass('fade-in');
                                $loadDate.removeClass('fade-out explode').addClass('fade-in');
                                $agentName.removeClass('fade-out explode').addClass('fade-in');
                                setTimeout(() => {
                                    $raterName.removeClass('fade-in');
                                    $loadDate.removeClass('fade-in');
                                    $agentName.removeClass('fade-in');
                                }, 500);
                            }, 500);
                        }
                    } catch (e) {
                        console.error('Error in clear button handler:', e);
                    }
                });

                // Handle Submit button click
                $('#submit-btn').on('click', function () {
                    try {
                        const $raterName = $('#rater-name');
                        const $loadDate = $('#load-date');
                        const $agentName = $('#agent-name');
                        const $overlay = $('#overlay');
                        const $progressBar = $('#progress-bar');
                        const $progressPercent = $('#progress-percent');
                        if ($raterName.length && $loadDate.length && $agentName.length && $overlay.length && $progressBar.length) {
                            const rater = $raterName.val();
                            if (!rater) {
                                alert('Please select a rater.');
                                return;
                            }
                            // Apply pulse animation
                            $raterName.addClass('pulse');
                            $loadDate.addClass('pulse');
                            $agentName.addClass('pulse');
                            $overlay.removeClass('hidden');
                            $progressBar.css('width', '0');

                            // Poll progress from backend
                            let progressInterval = setInterval(function () {
                                $.get('/get_progress', function (data) {
                                    const progress = data.progress;
                                    $progressPercent.text(`${progress}%`);
                                    $progressBar.css('width', `${(progress / 100) * 300}px`);
                                    if (progress >= 100) {
                                        clearInterval(progressInterval);
                                    }
                                }).fail(function () {
                                    console.error('Failed to fetch progress');
                                });
                            }, 200);

                            // Submit data to fetch CSV
                            $.ajax({
                                url: '/fetch_data',
                                method: 'POST',
                                contentType: 'application/json',
                                data: JSON.stringify({
                                    rater_name: rater,
                                    agent_names: JSON.stringify($agentName.val() || []),
                                    load_dates: JSON.stringify($loadDate.val() ? $loadDate.val().split(', ') : [])
                                }),
                                xhrFields: {
                                    responseType: 'blob'
                                },
                                success: function (data) {
                                    // Handle CSV download
                                    $raterName.removeClass('pulse').addClass('fade-out');
                                    $loadDate.removeClass('pulse').addClass('fade-out');
                                    $agentName.removeClass('pulse').addClass('fade-out');
                                    setTimeout(() => {
                                        const url = window.URL.createObjectURL(new Blob([data]));
                                        const a = document.createElement('a');
                                        a.href = url;
                                        a.download = 'data.csv';
                                        document.body.appendChild(a);
                                        a.click();
                                        document.body.removeChild(a);
                                        window.URL.revokeObjectURL(url);
                                        // Reset UI
                                        $raterName.removeClass('fade-out').addClass('fade-in');
                                        $loadDate.removeClass('fade-out').addClass('fade-in');
                                        $agentName.removeClass('fade-out').addClass('fade-in');
                                        $overlay.addClass('hidden');
                                        $progressBar.css('width', '0');
                                        setTimeout(() => {
                                            $raterName.removeClass('fade-in');
                                            $loadDate.removeClass('fade-in');
                                            $agentName.removeClass('fade-in');
                                        }, 500);
                                    }, 500);
                                },
                                error: function (xhr) {
                                    console.error('Error fetching data:', xhr);
                                    alert('Error fetching data: ' + (xhr.responseJSON ? xhr.responseJSON.error : 'Unknown error'));
                                    $overlay.addClass('hidden');
                                    $progressBar.css('width', '0');
                                    $raterName.removeClass('pulse');
                                    $loadDate.removeClass('pulse');
                                    $agentName.removeClass('pulse');
                                    clearInterval(progressInterval);
                                }
                            });
                        }
                    } catch (e) {
                        console.error('Error in submit button handler:', e);
                    }
                });

                // Initialize Bootstrap tooltips
                try {
                    $('[data-bs-toggle="tooltip"]').tooltip();
                    console.log('Tooltips initialized');
                } catch (e) {
                    console.error('Error initializing tooltips:', e);
                }
            } catch (e) {
                console.error('Global script error:', e);
            }
        });
    </script>
</body>
</html>
