```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Foundation Module - 50 BCQs</title>
    <style>
        :root {
            --primary: #2c3e50;
            --success: #27ae60;
            --error: #e74c3c;
            --light: #f4f7f6;
            --white: #ffffff;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--light);
            display: flex;
            justify-content: center;
            align-items: flex-start;
            min-height: 100vh;
            padding: 15px;
            margin: 0;
        }

        #quiz-container {
            background: var(--white);
            width: 100%;
            max-width: 650px;
            padding: 25px;
            border-radius: 16px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            margin-top: 20px;
        }

        .header {
            text-align: center;
            border-bottom: 2px solid var(--light);
            margin-bottom: 20px;
            padding-bottom: 10px;
        }

        #progress {
            color: var(--primary);
            font-size: 1.2rem;
            margin: 0;
        }

        #question-text {
            font-size: 1.25rem;
            color: #333;
            line-height: 1.5;
            margin-bottom: 20px;
        }

        .option {
            display: block;
            padding: 14px;
            margin: 10px 0;
            border: 2px solid #e1e8ed;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s ease;
            font-size: 1rem;
            background: var(--white);
        }

        .option:hover {
            border-color: var(--primary);
            background-color: #f8f9fa;
        }

        .option.correct {
            background-color: #d4edda !important;
            border-color: var(--success) !important;
            color: #155724;
            font-weight: bold;
        }

        .option.wrong {
            background-color: #f8d7da !important;
            border-color: var(--error) !important;
            color: #721c24;
        }

        .explanation {
            margin-top: 20px;
            padding: 15px;
            background-color: #eef2f7;
            border-left: 5px solid var(--primary);
            display: none;
            font-size: 0.95rem;
            line-height: 1.5;
            border-radius: 0 8px 8px 0;
        }

        .btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 16px;
            border-radius: 8px;
            width: 100%;
            margin-top: 20px;
            cursor: pointer;
            font-size: 1.1rem;
            font-weight: 600;
            display: none;
            transition: opacity 0.2s;
        }

        .btn:active {
            opacity: 0.8;
        }

        .result-screen {
            text-align: center;
            display: none;
        }

        .percentage {
            font-size: 4rem;
            font-weight: bold;
            color: var(--primary);
            margin: 20px 0;
        }

        #score-details {
            font-size: 1.2rem;
            color: #666;
            margin-bottom: 30px;
        }

        @media (max-width: 480px) {
            #quiz-container { padding: 15px; }
            #question-text { font-size: 1.1rem; }
            .percentage { font-size: 3rem; }
        }
    </style>
</head>
<body>

<div id="quiz-container">
    <div id="quiz-body">
        <div class="header">
            <h2 id="progress">Question 1/50</h2>
        </div>
        <h3 id="question-text">Loading question...</h3>
        <div id="options-container"></div>
        <div id="explanation-box" class="explanation"></div>
        <button id="next-btn" class="btn">Next Question</button>
    </div>

    <div id="result-screen" class="result-screen">
        <h2>Quiz Results</h2>
        <div class="percentage" id="final-score">0%</div>
        <p id="score-details"></p>
        <button onclick="location.reload()" class="btn" style="display:block">Restart Quiz</button>
    </div>
</div>

<script>
    const allQuestions = [
        { q: "The function of the condenser is to:", options: ["Enhance the image", "Enlarge the image", "Holds the slide", "Produce cone of light", "Produce light"], correct: 3, exp: "The condenser concentrates light into a cone to illuminate the specimen." },
        { q: "Compound tubulo-acinar glands are found in the:", options: ["Duodenum", "Large intestine", "Pancreas", "Small intestine", "Submandibular gland"], correct: 4, exp: "The submandibular gland is a classic example of this gland type." },
        { q: "All of the following are examples of hyaline cartilage EXCEPT:", options: ["Costa cartilage", "Epiglottis", "Nasal septum", "Thyroid cartilage", "Trachea"], correct: 1, exp: "The epiglottis is composed of elastic cartilage." },
        { q: "Oogonia reach their maximum number at which stage of human development?", options: ["5th month of fetal life", "At birth", "Early adulthood", "Early childhood", "Puberty"], correct: 0, exp: "Germ cells peak at roughly 7 million during the 5th month of gestation." },
        { q: "When does the secondary oocyte undergo completion of the second meiotic division?", options: ["At fertilization", "Before ovulation", "Just after fertilization", "Just after ovulation", "Prenatally"], correct: 0, exp: "Meiosis II is completed only if fertilization occurs." },
        { q: "The period of intrauterine life from 3rd week to 8th week is called:", options: ["Antenatal period", "Embryonic period", "Foetal period", "Infancy", "Postnatal period"], correct: 1, exp: "This timeframe marks the embryonic stage of development." },
        { q: "Which of the following is an example of short bone?", options: ["Carpals", "Clavicle", "Humerus", "Metacarpals", "Phalanges"], correct: 0, exp: "Carpals (wrist bones) are classified as short bones." },
        { q: "The muscle which stabilizes the joints of agonists is called:", options: ["Antagonist", "Fixator", "Prime mover", "Stabilizer", "Synergist"], correct: 1, exp: "A fixator stabilizes the origin of the prime mover." },
        { q: "Lumbar puncture in adults is performed between which of following vertebrae:", options: ["L1 & L2", "L2 & L3", "L3 & L4", "L4 & L5", "T12 & L1"], correct: 3, exp: "L4-L5 is the safest site as the spinal cord usually ends at L1-L2." },
        { q: "Regarding lymphatic system all are correct EXCEPT:", options: ["Afferent lymphatics enter lymph node at hilum", "Lymph is clear fluid containing WBCs", "Spleen has no afferent lymphatics", "Thoracic duct is the largest lymph vessel", "Tonsils are non-capsulated"], correct: 0, exp: "Afferent vessels enter the convex surface; only efferent vessels leave via the hilum." },
        { q: "Cell membrane is:", options: ["Entirely made of protein molecules", "Freely permeable to electrolytes", "Highly selectively permeable", "Permeable to fat soluble substances", "Semi-permeable in all cells"], correct: 3, exp: "The lipid bilayer allows fat-soluble substances to pass easily." },
        { q: "Which organelle in the cytoplasm constitutes the bulk of it?", options: ["Endoplasmic reticulum", "Golgi apparatus", "Lysosomes", "Mitochondria", "Ribosomes"], correct: 0, exp: "The ER network is the most extensive organelle system in the cytoplasm." },
        { q: "Which of the following processes does not exhibit 'saturation kinetics'?", options: ["Active transport", "Facilitated diffusion", "Na+ coupled active transport", "Na+-Ca2+ exchanger", "Simple diffusion"], correct: 4, exp: "Simple diffusion rate is only limited by the concentration gradient." },
        { q: "The potential at which net flux of an ion across the membrane is zero is called:", options: ["Electrotonic potential", "Equilibrium potential", "Resting membrane potential", "Spike potential", "Threshold potential"], correct: 1, exp: "This is the Nernst equilibrium potential for a specific ion." },
        { q: "Negative feedback mechanism which is beneficial for human is:", options: ["Birth of baby", "Clotting of blood", "Generation of nerve impulse", "Regulation of arterial pressure", "Uterine contraction during labor"], correct: 3, exp: "Arterial pressure regulation is a homeostatic negative feedback process." },
        { q: "The composition of intracellular fluid mainly differs from that of extracellular as it has:", options: ["Higher molar concentration of potassium", "Higher molar concentration of sodium", "Higher tonicity", "Lower tonicity", "Principal inorganic anions"], correct: 0, exp: "Potassium is the primary intracellular cation." },
        { q: "The rates of diffusion of a particle across a membrane will increase if:", options: ["The area of the membrane decreases", "The concentration gradient of the particle decreases", "The lipid solubility of the particle increases", "The size of the particle increases", "The thickness of the membrane increases"], correct: 2, exp: "Increased lipid solubility allows particles to pass the membrane faster." },
        { q: "In a nerve, the magnitude of the action potential overshoot is normally a function of the:", options: ["Diameter of the axon", "Extracellular sodium concentration", "Intracellular potassium concentration", "Magnitude of the stimulus", "Resting membrane potential"], correct: 1, exp: "Overshoot depends on the sodium equilibrium potential." },
        { q: "Nernst potential for K+ is:", options: ["+10mv", "+55mv", "+61mv", "-90mv", "-94mv"], correct: 4, exp: "The standard calculated Nernst potential for Potassium is -94mv." },
        { q: "Plateau potential is not seen in:", options: ["Cardiac muscle fibres", "Purkinje fibres of the heart", "Skeletal muscle fibres", "Smooth muscle fibres in gut", "Smooth muscle of uterus"], correct: 2, exp: "Skeletal muscles have sharp, brief action potentials without a plateau." },
        { q: "Which organelle is responsible for biosynthesis of proteins and lipoproteins?", options: ["Cytosol", "Endoplasmic Reticulum", "Golgi Apparatus", "Mitochondria", "Nucleus"], correct: 1, exp: "Rough ER handles proteins, while Smooth ER handles lipids/lipoproteins." },
        { q: "The following are the primary system that regulates the acid-base homeostasis acting regulatory mechanism:", options: ["Chemical buffer system", "Protein and renal mechanism", "Protein buffer system", "Renal mechanism", "Respiratory mechanism"], correct: 0, exp: "Chemical buffers are the first line of defense in pH regulation." },
        { q: "Which of the following is non reducing sugar?", options: ["Fructose", "Glucose", "Lactose", "Maltose", "Sucrose"], correct: 4, exp: "Sucrose has no free anomeric carbon to act as a reducing agent." },
        { q: "Which sugar is known as reference sugar?", options: ["Dihydroxyacetone", "Fructose", "Glucose", "Glyceraldehydes", "Maltose"], correct: 3, exp: "Glyceraldehyde is used to define D and L configurations." },
        { q: "Neuraminic acid is unstable and found in nature in the form of acylated derivatives known as:", options: ["Butyric acid", "Hydrochloric acid", "Palmitic acid", "Sialic acid", "Sulphuric acid"], correct: 3, exp: "Sialic acids are N- or O-acylated derivatives of neuraminic acid." },
        { q: "Glycosidic bond in sucrose is:", options: ["Alpha 1-2", "Alpha 1-4", "Alpha 1-6", "Beta 1-2", "Beta 1-4"], correct: 0, exp: "Sucrose is formed by a bond between glucose (alpha-1) and fructose (beta-2)." },
        { q: "Which of the following are basic amino acid?", options: ["Arginine and histidine", "Glycine and alanine", "Histidine and lysine", "Histidine, lysine and glycine", "Histidine, lysine, arginine"], correct: 4, exp: "Histidine, Lysine, and Arginine are the three basic amino acids." },
        { q: "Collagen is rich in:", options: ["Alanine and glycine", "Glutamate and aspartate", "Glutamate and glycine", "Glutamate and proline", "Proline and glycine"], correct: 4, exp: "Glycine and Proline are essential for the collagen triple helix." },
        { q: "Which of the following is present in the plasma but absent in the serum?", options: ["Albumin", "Fibrinogen", "Globulin", "Globulin and lecithin", "Lecithin"], correct: 1, exp: "Fibrinogen is converted to fibrin during clotting and is not in serum." },
        { q: "Dietary fats are transported as:", options: ["Chylomicrons", "Lipid globules", "Liposomes", "Oil droplets", "VLDL"], correct: 0, exp: "Chylomicrons carry exogenous lipids from the intestines." },
        { q: "Which of the following types of necrosis is observed in patient with myocardial infarction?", options: ["Caseous necrosis", "Coagulative necrosis", "Fibrinoid necrosis", "Gangrenous necrosis", "Liquefactive necrosis"], correct: 1, exp: "Ischemia in most organs (except the brain) leads to coagulative necrosis." },
        { q: "Which of the following is the feature of hypertrophy?", options: ["Increase in number of cells", "Occurs due to ischemia", "Occurs in labile cells", "Occurs in skeletal muscles due to weight lifting", "One cell type is replaced with other cell type"], correct: 3, exp: "Hypertrophy is the increase in cell size (e.g., muscle growth)." },
        { q: "The final executioner in the process of apoptosis after which there is no point of return is:", options: ["Activation of Caspases", "Apoptotic body", "Change in temperature", "Hypoxia", "Phagocytosis"], correct: 0, exp: "Caspases are the enzymes that dismantle the cell during apoptosis." },
        { q: "What is the role of the bacterial capsule in pathogenicity?", options: ["It has no effect on bacterial pathogenicity", "It helps the bacteria evade host immune response", "It involved in conjugation", "It produces toxins that damage host tissues", "It promotes the attachment of bacteria to host cells"], correct: 1, exp: "Capsules protect bacteria from phagocytosis." },
        { q: "Which of the following bacterial component is pyrogenic?", options: ["Capsule", "Dipicolinic acid", "Lipid A", "Peptidoglycan", "Teichoic acid"], correct: 2, exp: "Lipid A is the endotoxin component that causes fever." },
        { q: "Zero order kinetics can be defined as:", options: ["Constant amount of drug eliminated per unit time.", "Constant fraction of drug eliminated per unit time.", "Time taken by a drug to be eliminated by 100%.", "Zero amount of drug available in unchanged form.", "Zero amount of drug eliminated from body after distribution."], correct: 0, exp: "Elimination rate is constant regardless of drug concentration." },
        { q: "Through which of the following routes of drug administration, the drug can be easily administered with minimum irritation?", options: ["Intra muscular", "Intrathecal", "Oral", "Rectal", "Sub-cutaneous"], correct: 2, exp: "The oral route is generally the safest and least irritating." },
        { q: "Converted to active form after metabolism:", options: ["Prodrug", "Prototype drug", "Semi-synthetic drug"], correct: 0, exp: "A prodrug is inactive until metabolized in the body." },
        { q: "During transport of drug across the cell membrane, which of the following processes reversibly requires only carrier protein/molecule:", options: ["Active Transport", "Facilitated diffusion", "Phagocytosis", "Pinocytosis", "Simple diffusion"], correct: 1, exp: "Facilitated diffusion uses a carrier but no energy." },
        { q: "Health services must be shared equally by all people irrespective of their ability to pay. This refers to:", options: ["Appropriate health care", "Basic health care", "Comprehensive health care", "Equitable distribution of health care", "Primary health care"], correct: 3, exp: "Equitable distribution is a key principle of primary health care." },
        { q: "The study of mutual relationship between living organisms and their environment is known as:", options: ["Anthropology", "Archeology", "Biology", "Ecology", "Sociology"], correct: 3, exp: "Ecology focuses on organisms and their surroundings." },
        { q: "Which one of the following does not represent the submerged portion of the iceberg?", options: ["Carriers cub clinical cases", "Diagnosed cases under treatment", "Latent-Healthy population", "Pre-symptomatic cases", "Undiagnosed cases"], correct: 1, exp: "Diagnosed/clinical cases are the 'tip' of the iceberg." },
        { q: "Actions including promotion, prevention, and curative medicine in all aspects, is known as:", options: ["Health protection", "Primary prevention", "Public health", "Rehabilitation", "Specific protection"], correct: 2, exp: "Public health covers the full spectrum of health care." },
        { q: "Reducing complexities and disabilities after a disease has developed:", options: ["Primary prevention", "Primordial prevention", "Secondary prevention", "Specific prevention", "Tertiary prevention"], correct: 4, exp: "Tertiary prevention focuses on rehabilitation and disability limitation." },
        { q: "Health is multi-dimensional. The dimension that is the easiest to understand is:", options: ["Mental dimension", "Nutritional dimension", "Physical dimension", "Psychological dimension", "Vocational dimension"], correct: 2, exp: "Physical health involves observable anatomical and physiological states." },
        { q: "The Greek word Ethics means:", options: ["Behavior", "Custom", "Identity", "Interaction", "The nature"], correct: 1, exp: "Ethics comes from 'ethos', meaning custom or character." },
        { q: "Subject of Science 'Bio Ethics' is concerned with:", options: ["Duty of Doctor", "Duty of Nurse", "Duty of Pharmacist", "Ethical issue in bio medical research", "Interaction between man and nature"], correct: 3, exp: "Bioethics specifically addresses dilemmas in medical and life sciences." },
        { q: "All physical parts of computer which can be seen or touched are called as:", options: ["Firmware", "Hardware", "Live-ware", "Share ware", "Software"], correct: 1, exp: "Hardware refers to the physical machinery." },
        { q: "The term Desktop is well known for:", options: ["It is a bottom bar", "It is a folder for deleted files", "It is a front screen where all icons/folders are shown", "It is a start menu", "It is a top bar"], correct: 2, exp: "The desktop is the main workspace on a computer screen." },
        { q: "Which of the following is called as brain of a computer:", options: ["Central Processing Unit", "Central Programming Unit", "Common Processing Unit", "Control Programming unit"], correct: 0, exp: "The CPU handles all primary processing and logic." }
    ];

    let currentQuestions = [];
    let currentIndex = 0;
    let score = 0;
    let answered = false;

    function initQuiz() {
        // Randomize question order every time
        currentQuestions = [...allQuestions].sort(() => Math.random() - 0.5);
        currentIndex = 0;
        score = 0;
        document.getElementById('result-screen').style.display = 'none';
        document.getElementById('quiz-body').style.display = 'block';
        showQuestion();
    }

    function showQuestion() {
        answered = false;
        const q = currentQuestions[currentIndex];
        document.getElementById('progress').innerText = `Question ${currentIndex + 1} of ${currentQuestions.length}`;
        document.getElementById('question-text').innerText = q.q;
        document.getElementById('explanation-box').style.display = 'none';
        document.getElementById('next-btn').style.display = 'none';
        
        const container = document.getElementById('options-container');
        container.innerHTML = '';

        q.options.forEach((opt, index) => {
            const div = document.createElement('div');
            div.className = 'option';
            div.innerText = opt;
            div.onclick = () => handleSelection(div, index);
