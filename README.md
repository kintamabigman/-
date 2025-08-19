<!DOCTYPE html>
<html lang="ja" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>日本の食肉市場：10年間の動向インタラクティブ分析</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Warm Harmony -->
    <!-- Application Structure Plan: A thematic, single-page dashboard. The structure begins with a high-level overview of the three meat types, allowing users to grasp key trends instantly. Clicking on a meat type smoothly navigates to a detailed analysis section. This section uses a tab-like interface to switch between beef, pork, and chicken, facilitating easy comparison. This is followed by a dedicated section explaining the interconnected market drivers (economy, global affairs), which the report emphasizes as crucial. This user-flow—from summary to detail to underlying causes—is more intuitive for data exploration than the linear structure of the original report. -->
    <!-- Visualization & Content Choices: 
        - Report Info: Overall consumption ranking. Goal: Compare. Viz: Bar Chart. Interaction: Hover tooltips. Justification: Provides an immediate, clear snapshot of the current market hierarchy. Library: Chart.js.
        - Report Info: Domestic vs. Import ratios. Goal: Inform/Compare. Viz: Doughnut Charts. Interaction: Hover tooltips. Justification: Best way to visualize proportional data clearly for each meat type. Library: Chart.js.
        - Report Info: Impact of weak yen and "Meat Shock". Goal: Organize/Relationships. Viz: HTML/CSS flow diagrams. Interaction: Subtle hover effects. Justification: Visually simplifies the complex cause-and-effect chains described in the report, making them easier to understand than text alone. Method: Tailwind CSS.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Noto Sans JP', sans-serif;
            background-color: #FDFCF9;
            color: #3D3D3D;
        }
        .chart-container {
            position: relative;
            width: 100%;
            height: 280px;
            max-height: 300px;
        }
        @media (min-width: 640px) {
            .chart-container {
                height: 320px;
                max-height: 350px;
            }
        }
        .active-nav {
            background-color: #A34A28 !important;
            color: #FFFFFF !important;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
        }
        .factor-card {
            background-color: white;
            border-radius: 0.75rem;
            padding: 1.5rem;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.05), 0 2px 4px -2px rgb(0 0 0 / 0.05);
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .factor-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.07), 0 4px 6px -2px rgb(0 0 0 / 0.05);
        }
    </style>
</head>
<body class="antialiased">

    <header class="bg-white shadow-sm sticky top-0 z-50">
        <nav class="container mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <h1 class="text-xl md:text-2xl font-bold text-[#A34A28]">日本の食肉市場分析</h1>
                <div class="hidden sm:flex items-center space-x-4">
                    <a href="#overview" class="text-gray-600 hover:text-[#A34A28] transition">概観</a>
                    <a href="#analysis" class="text-gray-600 hover:text-[#A34A28] transition">畜種別分析</a>
                    <a href="#factors" class="text-gray-600 hover:text-[#A34A28] transition">市場要因</a>
                    <a href="#outlook" class="text-gray-600 hover:text-[#A34A28] transition">今後の展望</a>
                </div>
            </div>
        </nav>
    </header>

    <main class="container mx-auto px-4 sm:px-6 lg:px-8 py-8 md:py-12">

        <section id="intro" class="text-center mb-12 md:mb-16">
            <h2 class="text-3xl md:text-4xl font-bold mb-4">日本の食肉市場：10年間の動向</h2>
            <p class="max-w-3xl mx-auto text-gray-600">
                過去10年間、日本の食肉消費は増加傾向にありますが、その内訳は大きく変化しています。このページでは、牛肉、豚肉、鶏肉の動向と、その背景にある経済や国際情勢などの複合的な要因をインタラクティブに探ります。
            </p>
        </section>

        <section id="overview" class="mb-12 md:mb-16">
            <h3 class="text-2xl font-bold text-center mb-8">主要トレンド概観 (2021年度時点)</h3>
            <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
                <div class="factor-card text-center flex flex-col items-center">
                    <div class="text-6xl mb-3">🐄</div>
                    <h4 class="text-xl font-bold mb-2 text-[#A34A28]">牛肉</h4>
                    <p class="text-gray-600 mb-4 flex-grow">消費は横ばい。外食依存度が高く、円安と国際価格高騰で輸入は減少傾向。</p>
                    <a href="#analysis" onclick="showContent('beef')" class="mt-auto bg-[#A34A28] text-white py-2 px-4 rounded-lg hover:bg-opacity-80 transition">詳しく見る</a>
                </div>
                <div class="factor-card text-center flex flex-col items-center">
                    <div class="text-6xl mb-3">🐖</div>
                    <h4 class="text-xl font-bold mb-2 text-[#E6A4B4]">豚肉</h4>
                    <p class="text-gray-600 mb-4 flex-grow">安定した家計需要に支えられ消費は増加。食卓の主役としての地位を確立。</p>
                    <a href="#analysis" onclick="showContent('pork')" class="mt-auto bg-[#E6A4B4] text-white py-2 px-4 rounded-lg hover:bg-opacity-80 transition">詳しく見る</a>
                </div>
                <div class="factor-card text-center flex flex-col items-center">
                    <div class="text-6xl mb-3">🐓</div>
                    <h4 class="text-xl font-bold mb-2 text-[#F2C57C]">鶏肉</h4>
                    <p class="text-gray-600 mb-4 flex-grow">健康・節約志向を追い風に消費量トップへ。10年間で需要が急増。</p>
                    <a href="#analysis" onclick="showContent('chicken')" class="mt-auto bg-[#F2C57C] text-white py-2 px-4 rounded-lg hover:bg-opacity-80 transition">詳しく見る</a>
                </div>
            </div>
        </section>
        
        <section class="mb-12 md:mb-16">
             <h3 class="text-2xl font-bold text-center mb-8">一人当たり年間消費量の比較 (2021年度)</h3>
             <p class="text-center text-gray-600 mb-8 max-w-2xl mx-auto">
                2021年度には、一人当たりの食肉合計消費量は過去最高の33.8kgを記録しました。特に鶏肉の消費量が豚肉を上回り、市場の主要プレイヤーとしての地位を固めています。
             </p>
             <div class="chart-container max-w-3xl mx-auto">
                <canvas id="consumptionChart"></canvas>
            </div>
        </section>

        <section id="analysis" class="mb-12 md:mb-16">
            <h3 class="text-2xl font-bold text-center mb-8">畜種別 詳細分析</h3>
            <div class="flex justify-center mb-8 space-x-2 md:space-x-4">
                <button id="nav-beef" onclick="showContent('beef')" class="nav-btn bg-white text-gray-700 py-2 px-4 rounded-lg shadow-md hover:bg-gray-100 transition">🐄 牛肉</button>
                <button id="nav-pork" onclick="showContent('pork')" class="nav-btn bg-white text-gray-700 py-2 px-4 rounded-lg shadow-md hover:bg-gray-100 transition">🐖 豚肉</button>
                <button id="nav-chicken" onclick="showContent('chicken')" class="nav-btn bg-white text-gray-700 py-2 px-4 rounded-lg shadow-md hover:bg-gray-100 transition">🐓 鶏肉</button>
            </div>

            <div id="content-container" class="bg-white p-6 md:p-8 rounded-xl shadow-lg">
                <div id="content-beef" class="content-panel">
                    <div class="grid md:grid-cols-2 gap-8 items-center">
                        <div>
                            <h4 class="text-2xl font-bold mb-4 text-[#A34A28]">牛肉：外食需要と国際情勢の鍵</h4>
                            <p class="text-gray-600 mb-4">
                                牛肉消費の約6割は外食・中食が占めており、景気や社会情勢の影響を受けやすい特徴があります。コロナ禍では外食需要が減少しましたが、家庭での「内食」需要が増加し、市場全体を支えました。
                            </p>
                            <p class="text-gray-600">
                                一方、供給の約6割を輸入品に依存しているため、円安や海外の生産状況、国際的な需要増（特に中国）が価格と供給量に直接的な影響を与えます。近年はこれらの要因が重なり、「ミートショック」と呼ばれる価格高騰も発生しました。
                            </p>
                        </div>
                        <div class="chart-container">
                            <canvas id="beefDoughnutChart"></canvas>
                            <p class="text-center text-sm text-gray-500 mt-2">需給構造（国産 vs 輸入）</p>
                        </div>
                    </div>
                </div>
                <div id="content-pork" class="content-panel hidden">
                    <div class="grid md:grid-cols-2 gap-8 items-center">
                        <div>
                            <h4 class="text-2xl font-bold mb-4 text-[#E6A4B4]">豚肉：食卓を支える安定需要</h4>
                            <p class="text-gray-600 mb-4">
                                豚肉は消費の約6割が家計消費であり、「普段使い」の食材として安定した需要を誇ります。この強い内食需要が、消費量全体の着実な増加を牽引してきました。
                            </p>
                            <p class="text-gray-600">
                                国内生産基盤は強化されていますが、輸入品も一定の割合を占めます。牛肉と同様に円安や海外の価格高騰は輸入量減少の要因となり、結果として国産豚肉への需要シフトが見られます。
                            </p>
                        </div>
                        <div class="chart-container">
                            <canvas id="porkDoughnutChart"></canvas>
                             <p class="text-center text-sm text-gray-500 mt-2">需給構造（国産 vs 輸入）</p>
                        </div>
                    </div>
                </div>
                <div id="content-chicken" class="content-panel hidden">
                    <div class="grid md:grid-cols-2 gap-8 items-center">
                        <div>
                            <h4 class="text-2xl font-bold mb-4 text-[#F2C57C]">鶏肉：健康と節約志向の二刀流</h4>
                            <p class="text-gray-600 mb-4">
                                鶏肉は2012年に豚肉の消費量を上回り、トップに躍り出ました。その背景には「高タンパク・低カロリー」という健康志向と、他の食肉に比べて価格が安定していることによる「節約志向」の二つの大きなトレンドがあります。
                            </p>
                            <p class="text-gray-600">
                                サラダチキンのヒットなどが象徴するように、新たな需要を開拓し続けています。国内生産は10年連続で増加していますが、旺盛な需要を補うため、ブラジル産を中心に輸入も増加傾向にあります。
                            </p>
                        </div>
                        <div class="chart-container">
                            <canvas id="chickenDoughnutChart"></canvas>
                             <p class="text-center text-sm text-gray-500 mt-2">需給構造（国産 vs 輸入）</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="factors" class="mb-12 md:mb-16">
            <h3 class="text-2xl font-bold text-center mb-8">市場を動かす複合的要因</h3>
            <p class="text-center text-gray-600 mb-10 max-w-3xl mx-auto">
                食肉市場の動向は、単一の理由では説明できません。マクロ経済、国際情勢、国内政策といった要因が複雑に絡み合い、消費者の選択に影響を与えています。
            </p>
            <div class="grid md:grid-cols-1 lg:grid-cols-3 gap-8">
                <div class="factor-card">
                    <h4 class="text-xl font-bold mb-3">① マクロ経済：円安の影響</h4>
                    <p class="text-gray-600 mb-4">歴史的な円安は輸入品の価格を直接押し上げ、消費者の「生活防衛意識」を高めました。</p>
                    <div class="space-y-2 text-sm">
                        <div class="bg-gray-100 p-2 rounded-lg flex items-center">円安進行 📉</div>
                        <div class="text-center">↓</div>
                        <div class="bg-gray-100 p-2 rounded-lg flex items-center">輸入食肉の円建て価格が上昇 💸</div>
                        <div class="text-center">↓</div>
                        <div class="bg-gray-100 p-2 rounded-lg flex items-center">安価な鶏肉や国産品への需要シフト 🐔</div>
                    </div>
                </div>
                <div class="factor-card">
                    <h4 class="text-xl font-bold mb-3">② 国際情勢：「ミートショック」</h4>
                    <p class="text-gray-600 mb-4">複数の世界的要因が連鎖し、輸入価格が記録的に高騰しました。</p>
                     <div class="space-y-2 text-sm">
                        <div class="bg-gray-100 p-2 rounded-lg">海外の生産・供給網の混乱 (コロナ禍)</div>
                        <div class="text-center">+</div>
                        <div class="bg-gray-100 p-2 rounded-lg">飼料穀物・輸送費の高騰 (ウクライナ情勢)</div>
                        <div class="text-center">↓</div>
                        <div class="bg-red-100 text-red-800 p-2 rounded-lg font-bold">世界的な食肉価格の高騰 📈</div>
                    </div>
                </div>
                <div class="factor-card">
                    <h4 class="text-xl font-bold mb-3">③ 政策・制度：貿易協定</h4>
                    <p class="text-gray-600">TPP11などの貿易協定により、牛・豚肉の関税は段階的に引き下げられています。</p>
                    <p class="text-gray-600 mt-4">しかし、近年の市場では、関税引き下げの効果が円安という巨大なマクロ経済要因によって相殺され、その影響は限定的となっています。</p>
                </div>
            </div>
        </section>

        <section id="outlook" class="bg-white p-6 md:p-8 rounded-xl shadow-lg">
            <h3 class="text-2xl font-bold text-center mb-6">今後の展望と結論</h3>
            <div class="grid md:grid-cols-2 gap-8 items-center">
                <div>
                    <h4 class="font-bold text-lg mb-2">市場の二極化とサプライチェーンの強靭化</h4>
                    <p class="text-gray-600 mb-4">
                        今後は、高品質な和牛などが輸出市場で評価される一方、国内では価格を重視する消費者が安価な鶏肉や国産の交雑牛などを選ぶ「市場の二極化」が一層進む可能性があります。
                    </p>
                    <p class="text-gray-600">
                        不確実性の高い世界情勢に対応するためには、輸入先の多角化や、国内の飼料基盤強化などを通じて、グローバルなリスクに強い食肉サプライチェーンを構築することが不可欠です。
                    </p>
                </div>
                <div class="text-center">
                    <div class="text-5xl">🌏 ➡️ 🇯🇵</div>
                    <p class="mt-4 font-semibold text-gray-700">強靭な食料供給網の構築へ</p>
                </div>
            </div>
        </section>

    </main>
    
    <footer class="bg-gray-800 text-white mt-12 md:mt-16">
        <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-6 text-center text-sm">
            <p>&copy; 2024 食肉市場インタラクティブ分析. All rights reserved.</p>
            <p class="text-gray-400 mt-1">このページは提供されたレポートに基づき、教育目的で作成されました。</p>
        </div>
    </footer>

    <script>
        const contentPanels = document.querySelectorAll('.content-panel');
        const navButtons = document.querySelectorAll('.nav-btn');

        function showContent(meatType) {
            contentPanels.forEach(panel => {
                if (panel.id === `content-${meatType}`) {
                    panel.classList.remove('hidden');
                } else {
                    panel.classList.add('hidden');
                }
            });

            navButtons.forEach(button => {
                if (button.id === `nav-${meatType}`) {
                    button.classList.add('active-nav');
                } else {
                    button.classList.remove('active-nav');
                }
            });
        }

        document.addEventListener('DOMContentLoaded', () => {
            showContent('beef');

            const wrapText = (str, maxWidth) => {
                const words = str.split(' ');
                let lines = [];
                let currentLine = words[0];

                for (let i = 1; i < words.length; i++) {
                    if (currentLine.length + words[i].length + 1 < maxWidth) {
                        currentLine += ' ' + words[i];
                    } else {
                        lines.push(currentLine);
                        currentLine = words[i];
                    }
                }
                lines.push(currentLine);
                return lines;
            };
            
            Chart.defaults.font.family = "'Noto Sans JP', sans-serif";

            const consumptionData = {
                labels: ['鶏肉', '豚肉', '牛肉'],
                datasets: [{
                    label: '一人当たり年間消費量 (kg)',
                    data: [14.4, 13.2, 6.1],
                    backgroundColor: [
                        'rgba(242, 197, 124, 0.7)',
                        'rgba(230, 164, 180, 0.7)',
                        'rgba(163, 74, 40, 0.7)'
                    ],
                    borderColor: [
                        '#F2C57C',
                        '#E6A4B4',
                        '#A34A28'
                    ],
                    borderWidth: 2
                }]
            };
            
            new Chart(document.getElementById('consumptionChart'), {
                type: 'bar',
                data: consumptionData,
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    indexAxis: 'y',
                    plugins: {
                        legend: {
                            display: false
                        },
                        title: {
                            display: false,
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return `${context.dataset.label}: ${context.raw} kg`;
                                }
                            }
                        }
                    },
                    scales: {
                        x: {
                            beginAtZero: true,
                            title: {
                                display: true,
                                text: '消費量 (kg/年)'
                            }
                        }
                    }
                }
            });

            const createDoughnutChart = (canvasId, data, colors, label) => {
                new Chart(document.getElementById(canvasId), {
                    type: 'doughnut',
                    data: {
                        labels: ['国産', '輸入'],
                        datasets: [{
                            label: label,
                            data: data,
                            backgroundColor: colors,
                            borderColor: '#FFFFFF',
                            borderWidth: 2,
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: {
                                position: 'bottom',
                            },
                            title: {
                                display: true,
                                text: label,
                                font: {
                                    size: 16
                                }
                            },
                            tooltip: {
                                callbacks: {
                                    label: function(context) {
                                        return `${context.label}: ${context.raw}%`;
                                    }
                                }
                            }
                        }
                    }
                });
            };

            createDoughnutChart('beefDoughnutChart', [40, 60], ['#A34A28', '#D4A28E'], '牛肉の需給割合');
            createDoughnutChart('porkDoughnutChart', [50, 50], ['#E6A4B4', '#F0C8D2'], '豚肉の需給割合 (概算)');
            createDoughnutChart('chickenDoughnutChart', [70, 30], ['#F2C57C', '#F7DDAA'], '鶏肉の需給割合 (概算)');
        });
    </script>
</body>
</html>
