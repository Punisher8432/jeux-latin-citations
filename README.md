
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Memoria Latina - Jeu antique</title>

<style>
    * {
        box-sizing: border-box;
    }

    body {
        margin: 0;
        min-height: 100vh;
        font-family: Georgia, "Times New Roman", serif;
        color: #3d2514;
        background:
            radial-gradient(ellipse at center, #e8d1a1, #9b7545);
        padding: 25px;
    }

    header {
        text-align: center;
        margin-bottom: 25px;
    }

    h1 {
        font-size: clamp(30px, 5vw, 52px);
        color: #542f12;
        text-shadow: 2px 2px #c9a76b;
        margin-bottom: 5px;
    }

    header p {
        font-style: italic;
        font-size: 18px;
    }

    .stats {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        gap: 15px;
        margin: 20px 0;
    }

    .stat {
        background: #f0dfb8;
        border: 2px solid #805323;
        border-radius: 10px;
        padding: 12px 25px;
        font-size: 18px;
        box-shadow: 3px 3px 0 #70451f;
    }

    .jeu {
        max-width: 1100px;
        margin: auto;
        background: rgba(247, 231, 195, 0.92);
        border: 5px double #70451f;
        border-radius: 15px;
        padding: 25px;
        box-shadow: 0 10px 30px #4b2b14;
    }

    h2 {
        text-align: center;
        color: #633d19;
        margin-top: 5px;
    }

    .colonnes {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 25px;
    }

    .colonne h3 {
        text-align: center;
        background: #805323;
        color: #fff0cd;
        padding: 12px;
        border-radius: 8px;
    }

    .carte {
        width: 100%;
        min-height: 65px;
        margin: 8px 0;
        padding: 12px;
        font-family: Georgia, serif;
        font-size: 17px;
        color: #44290f;
        background: #f8e8c4;
        border: 2px solid #a77b42;
        border-radius: 8px;
        cursor: pointer;
        transition: 0.2s;
    }

    .carte:hover {
        background: #e6c58b;
        transform: translateY(-2px);
    }

    .carte.selection {
        background: #b88742;
        color: white;
        border: 3px solid #59340f;
    }

    .carte.correct {
        background: #687b42;
        color: white;
        border-color: #405226;
        cursor: default;
        opacity: 0.8;
    }

    .carte.erreur {
        background: #a43d2e;
        color: white;
    }

    .carte:disabled {
        cursor: default;
    }

    .message {
        min-height: 35px;
        text-align: center;
        font-size: 19px;
        font-weight: bold;
        margin: 20px 0;
    }

    .commandes {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        gap: 15px;
        margin-top: 20px;
    }

    .bouton {
        padding: 13px 25px;
        font-size: 17px;
        font-family: Georgia, serif;
        background: #70451f;
        color: #fff0cd;
        border: 2px solid #482a10;
        border-radius: 8px;
        cursor: pointer;
    }

    .bouton:hover {
        background: #99682f;
    }

    footer {
        text-align: center;
        margin-top: 25px;
        font-style: italic;
    }

    @media (max-width: 700px) {
        .colonnes {
            grid-template-columns: 1fr;
            gap: 10px;
        }

        .jeu {
            padding: 15px;
        }

        body {
            padding: 10px;
        }
    }
</style>
</head>

<body>

<header>
    <h1>🏛️ MEMORIA LATINA 🏛️</h1>
    <p>« Verba latina, sapientia antiqua »</p>
    <p>Retrouve les traductions des expressions latines !</p>
</header>

<div class="stats">
    <div class="stat">Points : <span id="score">0</span></div>
    <div class="stat">Erreurs : <span id="erreurs">0</span></div>
    <div class="stat">Restantes : <span id="restantes">20</span></div>
</div>

<div class="jeu">
    <h2>Relie chaque expression latine à sa traduction française</h2>

    <div class="colonnes">
        <div class="colonne">
            <h3>📜 Expressions latines</h3>
            <div id="latin"></div>
        </div>

        <div class="colonne">
            <h3>⚔️ Traductions françaises</h3>
            <div id="francais"></div>
        </div>
    </div>

    <div class="message" id="message">
        Sélectionne une expression latine, puis sa traduction.
    </div>

    <div class="commandes">
        <button class="bouton" onclick="nouvellePartie()">
            🔄 Nouvelle partie
        </button>

        <button class="bouton" onclick="afficherReponses()">
            📖 Voir les réponses
        </button>
    </div>
</div>

<footer>
    SPQR — Senatus Populusque Romanus
</footer>

<script>
const citations = [
    ["Carpe diem", "Cueille le jour présent"],
    ["Veni, vidi, vici", "Je suis venu, j'ai vu, j'ai vaincu"],
    ["Cogito, ergo sum", "Je pense, donc je suis"],
    ["Alea jacta est", "Le sort en est jeté"],
    ["Memento mori", "Souviens-toi que tu vas mourir"],
    ["In vino veritas", "Dans le vin, la vérité"],
    ["Errare humanum est", "L'erreur est humaine"],
    ["Ad vitam aeternam", "Pour la vie éternelle"],
    ["Dura lex, sed lex", "La loi est dure, mais c'est la loi"],
    ["Mens sana in corpore sano", "Un esprit sain dans un corps sain"],
    ["Amor vincit omnia", "L'amour triomphe de tout"],
    ["Audaces fortuna juvat", "La fortune sourit aux audacieux"],
    ["Homo homini lupus", "L'homme est un loup pour l'homme"],
    ["Tempus fugit", "Le temps passe vite"],
    ["In medias res", "Au milieu des choses"],
    ["Per aspera ad astra", "Par des chemins difficiles vers les étoiles"],
    ["Vox populi", "La voix du peuple"],
    ["Divide et impera", "Diviser pour régner"],
    ["Fortes fortuna juvat", "La fortune favorise les courageux"],
    ["Pacta sunt servanda", "Les accords doivent être respectés"]
];

let score = 0;
let erreurs = 0;
let selection = null;
let trouvees = [];
let ordreFrancais = [];

function melanger(tableau) {
    let copie = [...tableau];

    for (let i = copie.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [copie[i], copie[j]] = [copie[j], copie[i]];
    }

    return copie;
}

function nouvellePartie() {
    score = 0;
    erreurs = 0;
    selection = null;

    trouvees = Array(citations.length).fill(false);

    ordreFrancais = melanger(
        citations.map((_, index) => index)
    );

    document.getElementById("score").textContent = score;
    document.getElementById("erreurs").textContent = erreurs;
    document.getElementById("restantes").textContent = citations.length;

    document.getElementById("message").textContent =
        "Sélectionne une expression latine, puis sa traduction.";

    afficherCartes();
}

function afficherCartes() {
    const latin = document.getElementById("latin");
    const francais = document.getElementById("francais");

    latin.innerHTML = "";
    francais.innerHTML = "";

    citations.forEach((citation, index) => {
        const bouton = document.createElement("button");

        bouton.className = "carte";
        bouton.textContent = citation[0];

        if (trouvees[index]) {
            bouton.classList.add("correct");
            bouton.disabled = true;
        }

        if (selection === index) {
            bouton.classList.add("selection");
        }

        bouton.onclick = () => choisirLatin(index);

        latin.appendChild(bouton);
    });

    ordreFrancais.forEach(index => {
        const bouton = document.createElement("button");

        bouton.className = "carte";
        bouton.textContent = citations[index][1];

        if (trouvees[index]) {
            bouton.classList.add("correct");
            bouton.disabled = true;
        }

        bouton.onclick = () => choisirFrancais(index);

        francais.appendChild(bouton);
    });
}

function choisirLatin(index) {
    if (trouvees[index]) return;

    selection = index;

    document.getElementById("message").textContent =
        "Maintenant, choisis la traduction correspondante.";

    afficherCartes();
}

function choisirFrancais(index) {
    if (trouvees[index]) return;

    if (selection === null) {
        document.getElementById("message").textContent =
            "Choisis d'abord une expression latine !";
        return;
    }

    if (selection === index) {
        trouvees[index] = true;
        score += 10;

        document.getElementById("message").textContent =
            "✓ Exact ! " + citations[index][0] + " : " +
            citations[index][1];

        selection = null;

        document.getElementById("score").textContent = score;

        const restantes = trouvees.filter(x => !x).length;

        document.getElementById("restantes").textContent = restantes;

        afficherCartes();

        if (restantes === 0) {
            document.getElementById("message").textContent =
                "🏆 VICTOIRE ! Toutes les expressions sont trouvées !";
        }

    } else {
        erreurs++;

        document.getElementById("erreurs").textContent = erreurs;

        document.getElementById("message").textContent =
            "✗ Mauvaise réponse ! Essaie encore.";

        const boutons = document.querySelectorAll("#francais .carte");

        boutons.forEach(bouton => {
            if (bouton.textContent === citations[index][1]) {
                bouton.classList.add("erreur");

                setTimeout(() => {
                    bouton.classList.remove("erreur");
                }, 600);
            }
        });
    }
}

function afficherReponses() {
    let texte = "📖 CORRIGÉ DES 20 EXPRESSIONS\n\n";

    citations.forEach((citation, index) => {
        texte += (index + 1) + ". " +
            citation[0] + " = " + citation[1] + "\n";
    });

    alert(texte);
}

nouvellePartie();
</script>

</body>
</html>
