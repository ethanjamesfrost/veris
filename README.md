# veris
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Alvéole & Veris Residential | Q2 Engagement & Impact Report</title>
    <!-- Chosen Palette: Honeycomb Harmony (Warm yellows, dark grays, off-white) -->
    <!-- Application Structure Plan: Single-page dashboard. Top section for portfolio-wide KPI cards. Main section features interactive city-based filters (buttons) that dynamically update two charts (property comparison bar chart, city distribution donut chart) and a detailed, sortable data table. This structure provides an immediate overview while allowing for easy, intuitive drill-down and comparison by region, which is more user-friendly than static, separated tables. -->
    <!-- Visualization & Content Choices: Portfolio KPIs -> Inform -> Dynamic HTML stats -> JS update -> Quick overview. City Data -> Compare/Organize -> Interactive Bar/Donut Charts & Filtered Table -> Chart.js/Canvas & JS -> Allows for direct comparison between properties and regions. Detailed Metrics -> Inform -> Sortable HTML Table -> JS -> Provides granular data access. Confirmed NO SVG/Mermaid. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #FFFBEB; /* Amber 50 */
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin-left: auto;
            margin-right: auto;
            height: 400px;
            max-height: 50vh;
        }
        .filter-btn {
            transition: all 0.3s ease;
        }
        .filter-btn.active {
            background-color: #F59E0B; /* Amber 500 */
            color: white;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
        }
        .kpi-card {
            background-color: white;
            border-radius: 0.75rem;
            padding: 1.5rem;
            box-shadow: 0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
            text-align: center;
        }
        .kpi-value {
            font-size: 2.25rem;
            font-weight: 700;
            color: #B45309; /* Amber 700 */
        }
        .kpi-label {
            font-size: 0.875rem;
            color: #4B5563; /* Gray 600 */
            margin-top: 0.5rem;
        }
        th {
            cursor: pointer;
        }
    </style>
</head>
<body class="text-gray-800">

    <div class="container mx-auto p-4 md:p-8">
        <header class="text-center mb-12">
            <h1 class="text-4xl font-bold text-amber-800 mb-2">Alvéole & Veris Residential</h1>
            <h2 class="text-2xl text-gray-600">Q2 Engagement & Impact Report</h2>
            <p class="text-sm text-gray-500 mt-2">Report Date: June 5th, 2025 (Year-to-date data)</p>
        </header>

        <main>
            <!-- Portfolio Highlights Section -->
            <section id="portfolio-highlights" class="mb-12">
                <h3 class="text-2xl font-semibold text-amber-700 mb-6 text-center">Portfolio-Wide Impact</h3>
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4 md:gap-6">
                    <div class="kpi-card">
                        <div id="kpi-hives" class="kpi-value">30</div>
                        <div class="kpi-label">Honey Bee Hives</div>
                    </div>
                    <div class="kpi-card">
                        <div id="kpi-flowers" class="kpi-value">21.9B</div>
                        <div class="kpi-label">Flowers Pollinated Annually</div>
                    </div>
                    <div class="kpi-card">
                        <div id="kpi-honey" class="kpi-value">3k</div>
                        <div class="kpi-label">Jars of Honey</div>
                    </div>
                    <div class="kpi-card">
                        <div id="kpi-users" class="kpi-value">1.19k</div>
                        <div class="kpi-label">MyHive Unique Users</div>
                    </div>
                </div>
            </section>

            <!-- Interactive Data Explorer Section -->
            <section id="interactive-explorer" class="bg-white p-6 rounded-xl shadow-lg">
                <h3 class="text-2xl font-semibold text-amber-700 mb-6 text-center">Asset-Level Explorer</h3>
                <p class="text-center text-gray-600 max-w-3xl mx-auto mb-8">
                    Explore the performance of individual assets. Use the filters to view data by city, and click on chart elements for more details. The table below provides a comprehensive breakdown and can be sorted by clicking the column headers.
                </p>

                <!-- City Filters -->
                <div class="flex justify-center space-x-2 md:space-x-4 mb-8" id="city-filters">
                    <button class="filter-btn active px-4 py-2 bg-white text-amber-600 rounded-full font-semibold border border-amber-200 hover:bg-amber-100" data-city="all">All Cities</button>
                    <button class="filter-btn px-4 py-2 bg-white text-amber-600 rounded-full font-semibold border border-amber-200 hover:bg-amber-100" data-city="New York City">New York City</button>
                    <button class="filter-btn px-4 py-2 bg-white text-amber-600 rounded-full font-semibold border border-amber-200 hover:bg-amber-100" data-city="Boston">Boston</button>
                </div>

                <!-- Charts -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-8">
                    <div class="w-full">
                        <h4 class="text-xl font-semibold text-center text-amber-700 mb-4">MyHive Page Views by Property</h4>
                         <div class="chart-container h-96 md:h-[450px]">
                            <canvas id="engagementChart"></canvas>
                        </div>
                    </div>
                    <div class="w-full">
                        <h4 class="text-xl font-semibold text-center text-amber-700 mb-4">Honey Production (Lbs) by Property</h4>
                        <div class="chart-container h-96 md:h-[450px]">
                            <canvas id="honeyChart"></canvas>
                        </div>
                    </div>
                </div>

                <!-- Detailed Data Table -->
                <div class="overflow-x-auto">
                    <table class="min-w-full bg-white">
                        <thead class="bg-amber-500 text-white">
                            <tr>
                                <th class="p-3 text-left text-sm font-semibold tracking-wider" data-sort="name">Property Name</th>
                                <th class="p-3 text-left text-sm font-semibold tracking-wider" data-sort="city">City</th>
                                <th class="p-3 text-left text-sm font-semibold tracking-wider" data-sort="hives">Hives</th>
                                <th class="p-3 text-left text-sm font-semibold tracking-wider" data-sort="honey">Honey (lbs)</th>
                                <th class="p-3 text-left text-sm font-semibold tracking-wider" data-sort="workshops">Workshops Booked</th>
                                <th class="p-3 text-left text-sm font-semibold tracking-wider" data-sort="views">Page Views</th>
                                <th class="p-3 text-left text-sm font-semibold tracking-wider" data-sort="users">Unique Users</th>
                            </tr>
                        </thead>
                        <tbody id="property-table-body" class="divide-y divide-amber-100">
                            <!-- JS will populate this -->
                        </tbody>
                    </table>
                </div>
            </section>
        </main>

        <footer class="text-center mt-12 text-gray-500 text-sm">
            <p>&copy; 2025 Alvéole | Interactive Report for Veris Residential</p>
        </footer>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const assetData = [
                { name: '1100 Ave at Port Imperial', city: 'New York City', hives: 2, honey: 66, workshops: 1, views: 109, users: 63 },
                { name: 'Quarry Place', city: 'New York City', hives: 2, honey: 66, workshops: 1, views: 69, users: 46 },
                { name: 'Signature Place', city: 'New York City', hives: 2, honey: 66, workshops: 1, views: 174, users: 91 },
                { name: 'RiverHouse 9', city: 'New York City', hives: 2, honey: 66, workshops: 1, views: 85, users: 54 },
                { name: 'Haus25', city: 'New York City', hives: 2, honey: 66, workshops: 0, views: 205, users: 159 },
                { name: 'The Capstone', city: 'New York City', hives: 2, honey: 66, workshops: 1, views: 96, users: 66 },
                { name: 'The James', city: 'New York City', hives: 2, honey: 66, workshops: 0, views: 25, users: 10 },
                { name: 'The Upton', city: 'New York City', hives: 2, honey: 66, workshops: 1, views: 135, users: 79 },
                { name: 'BLVD475', city: 'New York City', hives: 2, honey: 66, workshops: 0, views: 125, users: 83 },
                { name: 'Liberty Towers', city: 'New York City', hives: 2, honey: 66, workshops: 1, views: 135, users: 98 },
                { name: 'The BLVD', city: 'New York City', hives: 2, honey: 66, workshops: 0, views: 226, users: 139 },
                { name: 'Soho Lofts', city: 'New York City', hives: 2, honey: 66, workshops: 0, views: 56, users: 43 },
                { name: 'Portside', city: 'Boston', hives: 2, honey: 66, workshops: 1, views: 175, users: 113 },
                { name: 'The Emery', city: 'Boston', hives: 2, honey: 66, workshops: 1, views: 99, users: 58 },
                { name: '145 Front', city: 'Boston', hives: 2, honey: 66, workshops: 1, views: 126, users: 87 }
            ];

            let currentSort = { column: 'name', direction: 'asc' };
            let currentCityFilter = 'all';

            const tableBody = document.getElementById('property-table-body');
            const filterButtons = document.querySelectorAll('.filter-btn');
            const tableHeaders = document.querySelectorAll('th[data-sort]');

            const engagementCtx = document.getElementById('engagementChart').getContext('2d');
            const honeyCtx = document.getElementById('honeyChart').getContext('2d');

            const createChart = (ctx, type, label, data, labels, backgroundColor, borderColor) => {
                return new Chart(ctx, {
                    type: type,
                    data: {
                        labels: labels,
                        datasets: [{
                            label: label,
                            data: data,
                            backgroundColor: backgroundColor,
                            borderColor: borderColor,
                            borderWidth: 1
                        }]
                    },
                    options: {
                        indexAxis: 'y',
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: {
                                display: false
                            },
                            tooltip: {
                                callbacks: {
                                    label: function(context) {
                                        let label = context.dataset.label || '';
                                        if (label) {
                                            label += ': ';
                                        }
                                        if (context.parsed.x !== null) {
                                            label += context.parsed.x;
                                        }
                                        return label;
                                    }
                                }
                            }
                        },
                        scales: {
                            x: {
                                beginAtZero: true,
                                grid: {
                                    color: '#F3F4F6'
                                }
                            },
                            y: {
                                grid: {
                                    display: false
                                },
                                ticks: {
                                    callback: function(value, index, values) {
                                        const label = this.getLabelForValue(value);
                                        return label.length > 16 ? label.substring(0, 16) + '...' : label;
                                    }
                                }
                            }
                        }
                    }
                });
            };

            const engagementChart = createChart(
                engagementCtx,
                'bar',
                'Page Views',
                [], [],
                'rgba(245, 158, 11, 0.6)',
                'rgba(245, 158, 11, 1)'
            );
            
            const honeyChart = createChart(
                honeyCtx,
                'bar',
                'Honey (lbs)',
                [], [],
                'rgba(217, 119, 6, 0.6)',
                'rgba(217, 119, 6, 1)'
            );

            const updateCharts = (filteredData) => {
                const sortedByViews = [...filteredData].sort((a, b) => b.views - a.views);
                const sortedByHoney = [...filteredData].sort((a, b) => b.honey - a.honey);

                engagementChart.data.labels = sortedByViews.map(d => d.name);
                engagementChart.data.datasets[0].data = sortedByViews.map(d => d.views);
                engagementChart.update();
                
                honeyChart.data.labels = sortedByHoney.map(d => d.name);
                honeyChart.data.datasets[0].data = sortedByHoney.map(d => d.honey);
                honeyChart.update();
            };

            const renderTable = (data) => {
                tableBody.innerHTML = '';
                data.forEach((item, index) => {
                    const row = document.createElement('tr');
                    row.className = index % 2 === 0 ? 'bg-amber-50' : 'bg-white';
                    row.innerHTML = `
                        <td class="p-3 text-sm text-gray-700">${item.name}</td>
                        <td class="p-3 text-sm text-gray-700">${item.city}</td>
                        <td class="p-3 text-sm text-gray-700">${item.hives}</td>
                        <td class="p-3 text-sm text-gray-700">${item.honey}</td>
                        <td class="p-3 text-sm text-gray-700">${item.workshops}</td>
                        <td class="p-3 text-sm text-gray-700">${item.views}</td>
                        <td class="p-3 text-sm text-gray-700">${item.users}</td>
                    `;
                    tableBody.appendChild(row);
                });
            };

            const sortData = (data, column, direction) => {
                return [...data].sort((a, b) => {
                    const valA = a[column];
                    const valB = b[column];
                    
                    if (typeof valA === 'string') {
                        return direction === 'asc' ? valA.localeCompare(valB) : valB.localeCompare(valA);
                    } else {
                        return direction === 'asc' ? valA - valB : valB - valA;
                    }
                });
            };

            const updateData = () => {
                let filteredData = assetData;
                if (currentCityFilter !== 'all') {
                    filteredData = assetData.filter(item => item.city === currentCityFilter);
                }
                const sorted = sortData(filteredData, currentSort.column, currentSort.direction);
                renderTable(sorted);
                updateCharts(filteredData);
            };

            filterButtons.forEach(button => {
                button.addEventListener('click', () => {
                    filterButtons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    currentCityFilter = button.dataset.city;
                    updateData();
                });
            });

            tableHeaders.forEach(header => {
                header.addEventListener('click', () => {
                    const column = header.dataset.sort;
                    const direction = (currentSort.column === column && currentSort.direction === 'asc') ? 'desc' : 'asc';
                    currentSort = { column, direction };
                    updateData();
                });
            });

            // Initial load
            updateData();
        });
    </script>
</body>
</html>
