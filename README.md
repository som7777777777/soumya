<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Transgenic Plants - Interactive Presentation</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #2ecc71 0%, #27ae60 50%, #16a085 100%);
            min-height: 100vh;
            overflow: hidden;
            position: relative;
        }

        .dna-animation {
            position: absolute;
            width: 100%;
            height: 100%;
            opacity: 0.1;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><path d="M10 10 Q 50 50 90 10 Q 50 50 10 90 Q 50 50 90 90" stroke="white" stroke-width="2" fill="none"/></svg>');
            animation: float 20s infinite ease-in-out;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(180deg); }
        }

        .presentation-container {
            position: relative;
            width: 100vw;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1;
        }

        .slide {
            width: 90%;
            max-width: 1000px;
            height: 80vh;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            padding: 40px;
            display: none;
            opacity: 0;
            transform: translateX(100px);
            transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            position: relative;
            overflow-y: auto;
        }

        .slide.active {
            display: block;
            opacity: 1;
            transform: translateX(0);
        }

        .slide h1 {
            font-size: 3.2em;
            color: #2c3e50;
            margin-bottom: 30px;
            text-align: center;
            background: linear-gradient(45deg, #27ae60, #2ecc71);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            animation: slideInFromTop 0.8s ease-out;
        }

        .slide h2 {
            font-size: 2.2em;
            color: #34495e;
            margin-bottom: 25px;
            text-align: center;
            animation: slideInFromTop 0.8s ease-out 0.2s both;
        }

        .slide h3 {
            font-size: 1.6em;
            color: #2c3e50;
            margin-bottom: 20px;
            border-left: 4px solid #27ae60;
            padding-left: 20px;
        }

        .slide p {
            font-size: 1.2em;
            line-height: 1.6;
            color: #555;
            margin-bottom: 20px;
            animation: fadeInUp 0.8s ease-out 0.4s both;
        }

        .slide ul {
            font-size: 1.1em;
            line-height: 1.8;
            color: #555;
            margin-bottom: 20px;
            animation: fadeInUp 0.8s ease-out 0.6s both;
        }

        .slide li {
            margin-bottom: 8px;
            padding-left: 20px;
            position: relative;
        }

        .slide li::before {
            content: "🧬";
            position: absolute;
            left: 0;
            font-size: 0.9em;
        }

        .interactive-element {
            background: linear-gradient(45deg, #27ae60, #2ecc71);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            font-size: 1em;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 8px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .interactive-element:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
            background: linear-gradient(45deg, #2ecc71, #27ae60);
        }

        .quiz-container {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin: 20px 0;
            border-left: 5px solid #27ae60;
        }

        .quiz-question {
            font-size: 1.2em;
            margin-bottom: 15px;
            color: #2c3e50;
            font-weight: 600;
        }

        .quiz-options {
            display: grid;
            gap: 10px;
            margin-bottom: 15px;
        }

        .quiz-option {
            padding: 10px 15px;
            background: white;
            border: 2px solid #ecf0f1;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .quiz-option:hover {
            border-color: #27ae60;
            background: #f0f8f0;
        }

        .quiz-option.correct {
            background: #d4edda;
            border-color: #27ae60;
        }

        .quiz-option.incorrect {
            background: #f8d7da;
            border-color: #e74c3c;
        }

        .progress-bar {
            width: 100%;
            height: 6px;
            background: #ecf0f1;
            border-radius: 3px;
            overflow: hidden;
            margin-bottom: 20px;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(45deg, #27ae60, #2ecc71);
            transition: width 0.3s ease;
        }

        .navigation {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 15px;
        }

        .nav-btn {
            background: linear-gradient(45deg, #27ae60, #2ecc71);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.9em;
        }

        .nav-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .nav-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        .slide-counter {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.9);
            padding: 8px 15px;
            border-radius: 15px;
            font-size: 0.9em;
            color: #2c3e50;
        }

        .examples-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin: 20px 0;
        }

        .example-card {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease;
            border-left: 4px solid #27ae60;
        }

        .example-card:hover {
            transform: translateY(-5px);
        }

        .highlight {
            background: linear-gradient(120deg, #a8e6cf 0%, #dcedc8 100%);
            padding: 2px 6px;
            border-radius: 4px;
            font-weight: 600;
        }

        @keyframes slideInFromTop {
            from {
                opacity: 0;
                transform: translateY(-30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .process-timeline {
            position: relative;
            padding: 20px 0;
        }

        .timeline-item {
            display: flex;
            align-items: center;
            margin: 15px 0;
            padding: 15px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
        }

        .timeline-number {
            background: #27ae60;
            color: white;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 15px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="dna-animation"></div>
    
    <div class="presentation-container">
        <div class="slide-counter">
            <span id="current-slide">1</span> / <span id="total-slides">8</span>
        </div>

        <div class="progress-bar">
            <div class="progress-fill" id="progress-fill"></div>
        </div>

        <!-- Slide 1: Title -->
        <div class="slide active">
            <h1>🧬 Transgenic Plants</h1>
            <h2>Genetic Engineering in Agriculture</h2>
            <p style="text-align: center; font-size: 1.4em; margin-top: 40px;">
                Exploring the science of <span class="highlight">genetically modified plants</span> and their impact on modern agriculture
            </p>
            <div style="text-align: center; margin-top: 50px;">
                <button class="interactive-element" onclick="showFunFact()">🌱 Did You Know?</button>
            </div>
            <div id="fun-fact" style="text-align: center; margin-top: 30px; font-style: italic; color: #27ae60; display: none;">
                The first transgenic plant was created in 1983 - a tobacco plant resistant to antibiotics!
            </div>
        </div>

        <!-- Slide 2: What are Transgenic Plants? -->
        <div class="slide">
            <h2>What are Transgenic Plants?</h2>
            <p>Transgenic plants are organisms that have been <span class="highlight">genetically modified</span> by introducing genes from other species into their genome.</p>
            
            <h3>Key Characteristics:</h3>
            <ul>
                <li>Contain DNA from different species</li>
                <li>Express new traits not found in nature</li>
                <li>Created through genetic engineering techniques</li>
                <li>Designed to solve specific agricultural problems</li>
            </ul>

            <div style="text-align: center; margin-top: 30px;">
                <button class="interactive-element" onclick="toggleDefinition()">🔬 Technical Definition</button>
            </div>
            <div id="technical-def" style="display: none; margin-top: 20px; padding: 20px; background: #f8f9fa; border-radius: 10px;">
                <strong>Technical Definition:</strong> A transgenic plant is one that has been transformed with genetic material from another organism using recombinant DNA technology, resulting in the stable integration and expression of foreign genes.
            </div>
        </div>

        <!-- Slide 3: Methods of Creation -->
        <div class="slide">
            <h2>Methods of Creating Transgenic Plants</h2>
            
            <div class="process-timeline">
                <div class="timeline-item">
                    <div class="timeline-number">1</div>
                    <div>
                        <h3>Agrobacterium-mediated Transformation</h3>
                        <p>Uses natural DNA transfer ability of Agrobacterium bacteria</p>
                    </div>
                </div>
                
                <div class="timeline-item">
                    <div class="timeline-number">2</div>
                    <div>
                        <h3>Gene Gun (Biolistics)</h3>
                        <p>DNA-coated particles shot into plant cells at high velocity</p>
                    </div>
                </div>
                
                <div class="timeline-item">
                    <div class="timeline-number">3</div>
                    <div>
                        <h3>Electroporation</h3>
                        <p>Electric pulses create pores in cell membranes for DNA entry</p>
                    </div>
                </div>
                
                <div class="timeline-item">
                    <div class="timeline-number">4</div>
                    <div>
                        <h3>Protoplast Fusion</h3>
                        <p>Fusion of cells with removed cell walls</p>
                    </div>
                </div>
            </div>

            <div style="text-align: center; margin-top: 20px;">
                <button class="interactive-element" onclick="showMethodDetails()">📊 Method Comparison</button>
            </div>
        </div>

        <!-- Slide 4: Examples of Transgenic Plants -->
        <div class="slide">
            <h2>Examples of Transgenic Plants</h2>
            
            <div class="examples-grid">
                <div class="example-card">
                    <h3>🌽 Bt Corn</h3>
                    <p><strong>Gene:</strong> Bacillus thuringiensis</p>
                    <p><strong>Trait:</strong> Insect resistance</p>
                    <p><strong>Benefit:</strong> Reduced pesticide use</p>
                </div>
                
                <div class="example-card">
                    <h3>🌾 Golden Rice</h3>
                    <p><strong>Gene:</strong> Beta-carotene synthesis</p>
                    <p><strong>Trait:</strong> Vitamin A production</p>
                    <p><strong>Benefit:</strong> Combats vitamin A deficiency</p>
                </div>
                
                <div class="example-card">
                    <h3>🥬 Roundup Ready Soybeans</h3>
                    <p><strong>Gene:</strong> Herbicide resistance</p>
                    <p><strong>Trait:</strong> Glyphosate tolerance</p>
                    <p><strong>Benefit:</strong> Simplified weed control</p>
                </div>
                
                <div class="example-card">
                    <h3>🍅 Flavr Savr Tomato</h3>
                    <p><strong>Gene:</strong> Antisense polygalacturonase</p>
                    <p><strong>Trait:</strong> Delayed ripening</p>
                    <p><strong>Benefit:</strong> Extended shelf life</p>
                </div>
            </div>

            <div style="text-align: center; margin-top: 20px;">
                <button class="interactive-element" onclick="showMoreExamples()">🔍 More Examples</button>
            </div>
        </div>

        <!-- Slide 5: Applications -->
        <div class="slide">
            <h2>Applications of Transgenic Plants</h2>
            
            <h3>🌱 Agricultural Applications</h3>
            <ul>
                <li>Pest and disease resistance</li>
                <li>Herbicide tolerance</li>
                <li>Improved nutritional content</li>
                <li>Enhanced crop yield</li>
                <li>Stress tolerance (drought, salt, cold)</li>
            </ul>

            <h3>🔬 Research Applications</h3>
            <ul>
                <li>Gene function studies</li>
                <li>Protein production</li>
                <li>Pharmaceutical compounds</li>
                <li>Biodegradable plastics</li>
            </ul>

            <h3>🌍 Environmental Applications</h3>
            <ul>
                <li>Phytoremediation</li>
                <li>Reduced chemical pesticide use</li>
                <li>Carbon sequestration</li>
                <li>Biofuel production</li>
            </ul>
        </div>

        <!-- Slide 6: Benefits and Risks -->
        <div class="slide">
            <h2>Benefits and Risks</h2>
            
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 30px; margin-top: 30px;">
                <div>
                    <h3 style="color: #27ae60;">✅ Benefits</h3>
                    <ul>
                        <li>Increased crop yields</li>
                        <li>Improved nutritional value</li>
                        <li>Reduced pesticide use</li>
                        <li>Enhanced food security</li>
                        <li>Climate adaptability</li>
                        <li>Medical applications</li>
                    </ul>
                </div>
                
                <div>
                    <h3 style="color: #e74c3c;">⚠️ Concerns</h3>
                    <ul>
                        <li>Environmental impact</li>
                        <li>Gene flow to wild species</li>
                        <li>Potential allergenicity</li>
                        <li>Ethical considerations</li>
                        <li>Economic dependency</li>
                        <li>Regulatory challenges</li>
                    </ul>
                </div>
            </div>

            <div style="text-align: center; margin-top: 30px;">
                <button class="interactive-element" onclick="toggleRiskAnalysis()">📊 Risk-Benefit Analysis</button>
            </div>
            <div id="risk-analysis" style="display: none; margin-top: 20px; padding: 20px; background: #fff3cd; border-radius: 10px;">
                <strong>Scientific Consensus:</strong> Most scientific organizations agree that approved transgenic crops are as safe as conventional crops, but continued monitoring and research are essential.
            </div>
        </div>

        <!-- Slide 7: Quiz -->
        <div class="slide">
            <h2>Test Your Knowledge! 🧠</h2>
            
            <div class="quiz-container">
                <div class="quiz-question">
                    What was the first transgenic plant ever created?
                </div>
                <div class="quiz-options">
                    <div class="quiz-option" onclick="selectAnswer(this, false)">Corn with Bt gene</div>
                    <div class="quiz-option" onclick="selectAnswer(this, true)">Tobacco with antibiotic resistance</div>
                    <div class="quiz-option" onclick="selectAnswer(this, false)">Golden Rice</div>
                    <div class="quiz-option" onclick="selectAnswer(this, false)">Roundup Ready Soybeans</div>
                </div>
                <div id="quiz-feedback" style="margin-top: 15px; font-weight: bold;"></div>
            </div>

            <div class="quiz-container">
                <div class="quiz-question">
                    Which method uses bacteria to transfer genes into plants?
                </div>
                <div class="quiz-options">
                    <div class="quiz-option" onclick="selectAnswer2(this, true)">Agrobacterium-mediated transformation</div>
                    <div class="quiz-option" onclick="selectAnswer2(this, false)">Gene gun</div>
                    <div class="quiz-option" onclick="selectAnswer2(this, false)">Electroporation</div>
                    <div class="quiz-option" onclick="selectAnswer2(this, false)">Protoplast fusion</div>
                </div>
                <div id="quiz-feedback2" style="margin-top: 15px; font-weight: bold;"></div>
            </div>

            <div style="text-align: center; margin-top: 30px;">
                <button class="interactive-element" onclick="showQuizResults()">📋 Show Results</button>
            </div>
        </div>

        <!-- Slide 8: Future Prospects -->
        <div class="slide">
            <h2>Future Prospects</h2>
            
            <h3>🚀 Emerging Technologies</h3>
            <ul>
                <li><strong>CRISPR-Cas9:</strong> Precise gene editing</li>
                <li><strong>Gene drives:</strong> Controlling inheritance patterns</li>
                <li><strong>Synthetic biology:</strong> Designing new biological systems</li>
                <li><strong>Epigenome editing:</strong> Modifying gene expression without changing DNA</li>
            </ul>

            <h3>🌟 Future Applications</h3>
            <ul>
                <li>Climate change adaptation</li>
                <li>Personalized nutrition</li>
                <li>Pharmaceutical production in plants</li>
                <li>Sustainable agriculture</li>
                <li>Space agriculture</li>
            </ul>

            <div style="text-align: center; margin-top: 30px; padding: 20px; background: linear-gradient(45deg, #f8f9fa, #e9ecef); border-radius: 15px;">
                <h3 style="color: #27ae60;">Thank You! 🌱</h3>
                <p style="font-size: 1.2em; margin-top: 10px;">Questions & Discussion</p>
            </div>
        </div>
    </div>

    <div class="navigation">
        <button class="nav-btn" id="prev-btn" onclick="previousSlide()" disabled>← Previous</button>
        <button class="nav-btn" id="next-btn" onclick="nextSlide()">Next →</button>
    </div>

    <script>
        let currentSlide = 0;
        const slides = document.querySelectorAll('.slide');
        const totalSlides = slides.length;
        
        document.getElementById('total-slides').textContent = totalSlides;
        
        function showSlide(n) {
            slides[currentSlide].classList.remove('active');
            currentSlide = n;
            slides[currentSlide].classList.add('active');
            
            document.getElementById('current-slide').textContent = currentSlide + 1;
            document.getElementById('progress-fill').style.width = ((currentSlide + 1) / totalSlides) * 100 + '%';
            
            document.getElementById('prev-btn').disabled = currentSlide === 0;
            document.getElementById('next-btn').disabled = currentSlide === totalSlides - 1;
        }
        
        function nextSlide() {
            if (currentSlide < totalSlides - 1) {
                showSlide(currentSlide + 1);
            }
        }
        
        function previousSlide() {
            if (currentSlide > 0) {
                showSlide(currentSlide - 1);
            }
        }
        
        // Keyboard navigation
        document.addEventListener('keydown', function(e) {
            if (e.key === 'ArrowRight') nextSlide();
            if (e.key === 'ArrowLeft') previousSlide();
        });
        
        // Interactive elements
        function showFunFact() {
            const element = document.getElementById('fun-fact');
            element.style.display = element.style.display === 'none' ? 'block' : 'none';
        }
        
        function toggleDefinition() {
            const element = document.getElementById('technical-def');
            element.style.display = element.style.display === 'none' ? 'block' : 'none';
        }
        
        function showMethodDetails() {
            alert('Agrobacterium-mediated: Most common, works well with dicots\nGene Gun: Works with all plant types, less precise\nElectroporation: Good for protoplasts, requires cell wall removal\nProtoplast Fusion: Allows large DNA transfer, labor intensive');
        }
        
        function showMoreExamples() {
            alert('Additional Examples:\n• Virus-resistant papaya\n• Drought-tolerant wheat\n• Iron-enriched beans\n• Caffeine-free coffee\n• Allergen-free peanuts');
        }
        
        function toggleRiskAnalysis() {
            const element = document.getElementById('risk-analysis');
            element.style.display = element.style.display === 'none' ? 'block' : 'none';
        }
        
        function selectAnswer(element, isCorrect) {
            const options = element.parentElement.querySelectorAll('.quiz-option');
            options.forEach(opt => opt.classList.remove('correct', 'incorrect'));
            
            if (isCorrect) {
                element.classList.add('correct');
                document.getElementById('quiz-feedback').innerHTML = '✅ Correct! The first transgenic plant was created in 1983.';
                document.getElementById('quiz-feedback').style.color = '#27ae60';
            } else {
                element.classList.add('incorrect');
                document.getElementById('quiz-feedback').innerHTML = '❌ Incorrect. The first transgenic plant was tobacco with antibiotic resistance.';
                document.getElementById('quiz-feedback').style.color = '#e74c3c';
            }
        }
        
        function selectAnswer2(element, isCorrect) {
            const options = element.parentElement.querySelectorAll('.quiz-option');
            options.forEach(opt => opt.classList.remove('correct', 'incorrect'));
            
            if (isCorrect) {
                element.classList.add('correct');
                document.getElementById('quiz-feedback2').innerHTML = '✅ Correct! Agrobacterium naturally transfers DNA to plants.';
                document.getElementById('quiz-feedback2').style.color = '#27ae60';
            } else {
                element.classList.add('incorrect');
                document.getElementById('quiz-feedback2').innerHTML = '❌ Incorrect. Agrobacterium-mediated transformation uses bacteria.';
                document.getElementById('quiz-feedback2').style.color = '#e74c3c';
            }
        }
        
        function showQuizResults() {
            const correct1 = document.querySelector('#quiz-feedback').innerHTML.includes('✅');
            const correct2 = document.querySelector('#quiz-feedback2').innerHTML.includes('✅');
            const score = (correct1 ? 1 : 0) + (correct2 ? 1 : 0);
            
            alert(`Quiz Results:\nScore: ${score}/2\n${score === 2 ? 'Excellent work! 🌟' : score === 1 ? 'Good effort! 👍' : 'Keep studying! 📚'}`);
        }
    </script>
</body>
</html>
