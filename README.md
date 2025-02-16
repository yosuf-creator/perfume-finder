<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Perfume Finder</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f0f4f8;
            margin: 0;
            padding: 20px;
            color: #333;
        }

        h1 {
            text-align: center;
            color: #5d5c61;
            font-size: 2rem;
            margin-bottom: 20px;
        }

        .form-container {
            background-color: #fff;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            max-width: 600px;
            margin: auto;
        }

        label {
            font-size: 1.1rem;
            margin-bottom: 8px;
            font-weight: bold;
            color: #555;
        }

        select, input[type="number"], input[type="text"], input[type="checkbox"] {
            width: 100%;
            padding: 12px;
            margin: 8px 0 20px 0;
            border-radius: 8px;
            border: 1px solid #ccc;
            font-size: 1rem;
        }

        .checkbox-container {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
            margin-bottom: 20px;
        }

        .checkbox-container label {
            display: flex;
            align-items: center;
            font-size: 1rem;
            color: #666;
        }

        .checkbox-container input[type="checkbox"] {
            margin-right: 10px;
        }

        .perfume-suggestions {
            margin-top: 40px;
            text-align: center;
        }

        .perfume-item {
            margin-bottom: 20px;
            font-size: 1.1rem;
        }

        .perfume-name {
            color: #007bff;
            text-decoration: none;
            font-weight: bold;
            font-size: 1.2rem;
        }

        .perfume-name:hover {
            text-decoration: underline;
            color: #0056b3;
        }

        .loading {
            font-size: 1.1rem;
            color: #888;
        }

        .no-results {
            font-size: 1.1rem;
            color: #d9534f;
        }

        .footer {
            margin-top: 40px;
            text-align: center;
            color: #555;
        }

        .footer a {
            color: #007bff;
            text-decoration: none;
        }

        .footer a:hover {
            text-decoration: underline;
        }

        @media (max-width: 600px) {
            .checkbox-container {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
</head>
<body>

    <h1>Let’s Find Your Perfect Perfume!</h1>

    <div class="form-container">
        <form id="perfumeForm">
            <label for="gender">What’s your gender?</label>
            <select id="gender">
                <option value="female">Female</option>
                <option value="male">Male</option>
                <option value="unisex">Unisex</option>
            </select>

            <label for="budget">What’s your budget? (in AED)</label>
            <input type="number" id="budget" placeholder="Enter your budget" min="1">

            <label for="favoriteNotes">What are your favorite notes? (Pick as many as you like)</label>
            <div class="checkbox-container" id="favoriteNotes">
                <label><input type="checkbox" value="amber"> Amber</label>
                <label><input type="checkbox" value="vanilla"> Vanilla</label>
                <label><input type="checkbox" value="rose"> Rose</label>
                <label><input type="checkbox" value="jasmine"> Jasmine</label>
                <label><input type="checkbox" value="lavender"> Lavender</label>
                <label><input type="checkbox" value="sandalwood"> Sandalwood</label>
                <label><input type="checkbox" value="bergamot"> Bergamot</label>
                <label><input type="checkbox" value="musk"> Musk</label>
                <label><input type="checkbox" value="leather"> Leather</label>
                <label><input type="checkbox" value="patchouli"> Patchouli</label>
                <label><input type="checkbox" value="cedarwood"> Cedarwood</label>
                <label><input type="checkbox" value="citrus"> Citrus</label>
                <label><input type="checkbox" value="oakmoss"> Oakmoss</label>
                <label><input type="checkbox" value="vetiver"> Vetiver</label>
                <label><input type="checkbox" value="tonka"> Tonka Bean</label>
                <label><input type="checkbox" value="pepper"> Pepper</label>
            </div>
        </form>
    </div>

    <div class="perfume-suggestions">
        <h2>Your Perfume Suggestions</h2>
        <div id="suggestions">
            <p class="loading">We’re finding the best perfumes for you...</p>
        </div>
    </div>

    <div class="footer">
        <p>Need more help? <a href="mailto:support@perfume.com">Contact us</a></p>
    </div>

    <script>
        const form = document.getElementById('perfumeForm');
        const suggestionsContainer = document.getElementById('suggestions');

        const perfumes = [
            { name: "Chanel No. 5", link: "https://www.google.com/search?q=Chanel+No.+5+perfume", notes: ["rose", "amber"], brand: "Chanel", gender: "female" },
            { name: "Dior Sauvage", link: "https://www.google.com/search?q=Dior+Sauvage+perfume", notes: ["pepper", "amber"], brand: "Dior", gender: "male" },
            { name: "Tom Ford Black Orchid", link: "https://www.google.com/search?q=Tom+Ford+Black+Orchid+perfume", notes: ["orchid", "patchouli"], brand: "Tom Ford", gender: "unisex" },
            { name: "Yves Saint Laurent Libre", link: "https://www.google.com/search?q=Yves+Saint+Laurent+Libre+perfume", notes: ["lavender", "vanilla"], brand: "Yves Saint Laurent", gender: "female" },
            { name: "Creed Aventus", link: "https://www.google.com/search?q=Creed+Aventus+perfume", notes: ["bergamot", "musk"], brand: "Creed", gender: "male" },
            { name: "Gucci Bloom", link: "https://www.google.com/search?q=Gucci+Bloom+perfume", notes: ["jasmine", "tuberose"], brand: "Gucci", gender: "female" },
            { name: "Montblanc Explorer", link: "https://www.google.com/search?q=Montblanc+Explorer+perfume", notes: ["bergamot", "patchouli"], brand: "Montblanc", gender: "male" },
            { name: "Paco Rabanne Invictus", link: "https://www.google.com/search?q=Paco+Rabanne+Invictus+perfume", notes: ["grapefruit", "musk"], brand: "Paco Rabanne", gender: "male" },
            { name: "Bvlgari Man in Black", link: "https://www.google.com/search?q=Bvlgari+Man+in+Black+perfume", notes: ["spicy", "amber"], brand: "Bvlgari", gender: "male" },
            { name: "Calvin Klein Euphoria", link: "https://www.google.com/search?q=Calvin+Klein+Euphoria+perfume", notes: ["pomegranate", "amber"], brand: "Calvin Klein", gender: "unisex" },
            { name: "Givenchy Gentlemen Only", link: "https://www.google.com/search?q=Givenchy+Gentlemen+Only+perfume", notes: ["spicy", "wood"], brand: "Givenchy", gender: "male" },
            { name: "Armani Code", link: "https://www.google.com/search?q=Armani+Code+perfume", notes: ["tonka", "leather"], brand: "Armani", gender: "male" },
            { name: "Hermès Terre d’Hermès", link: "https://www.google.com/search?q=Hermès+Terre+d’Hermès+perfume", notes: ["citrus", "vetiver"], brand: "Hermès", gender: "male" },
            { name: "Prada L’Homme", link: "https://www.google.com/search?q=Prada+L’Homme+perfume", notes: ["iris", "amber"], brand: "Prada", gender: "male" },
            { name: "Tom Ford Oud Wood", link: "https://www.google.com/search?q=Tom+Ford+Oud+Wood+perfume", notes: ["oud", "spices"], brand: "Tom Ford", gender: "unisex" },
            { name: "Chanel Bleu de Chanel", link: "https://www.google.com/search?q=Chanel+Bleu+de+Chanel+perfume", notes: ["citrus", "sandalwood"], brand: "Chanel", gender: "male" },
            { name: "Diptyque Philosykos", link: "https://www.google.com/search?q=Diptyque+Philosykos+perfume", notes: ["fig", "cedarwood"], brand: "Diptyque", gender: "unisex" }
        ];

        form.addEventListener('change', function() {
            const gender = document.getElementById('gender').value;
            const favoriteNotes = Array.from(document.querySelectorAll('#favoriteNotes input:checked')).map(checkbox => checkbox.value);

            // Show loading message
            suggestionsContainer.innerHTML = '<p class="loading">Hold on, we’re finding your perfect perfumes...</p>';

            // Filter perfumes based on gender and selected notes
            const filteredPerfumes = perfumes.filter(perfume => {
                const matchesNotes = favoriteNotes.some(note => perfume.notes.includes(note));
                const matchesGender = (perfume.gender === gender || perfume.gender === "unisex");
                return matchesNotes && matchesGender;
            });

            // Display results
            suggestionsContainer.innerHTML = '';
            if (filteredPerfumes.length === 0) {
                suggestionsContainer.innerHTML = '<p class="no-results">Oops, no perfumes match your criteria. Try selecting different notes!</p>';
            } else {
                filteredPerfumes.forEach(perfume => {
                    const perfumeItem = document.createElement('div');
                    perfumeItem.classList.add('perfume-item');
                    perfumeItem.innerHTML = `<a class="perfume-name" href="${perfume.link}" target="_blank">${perfume.name}</a>`;
                    suggestionsContainer.appendChild(perfumeItem);
                });
            }
        });
    </script>

</body>
</html>
