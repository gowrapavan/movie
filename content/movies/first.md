---
title: "Gowra Streaming"
params:
  synopsis: "An exciting adventure that takes you through the realms of imagination."
image: "/images/my-first-movie-poster.jpg"  # Specify the path to the image here
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #1e1e1e; /* Dark background for contrast */
            color: #f4f4f4; /* Light text for readability */
        }
        h1 {
            color: #ffffff; /* Header color */
            text-align: center; /* Centered header */
            margin-bottom: 20px;
        }
        p {
            text-align: center; /* Centered paragraph */
            margin-bottom: 20px;
        }
        .search-container {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
        }
        input[type="text"] {
            padding: 15px;
            width: 300px;
            border: none;
            border-radius: 5px 0 0 5px; /* Rounded left corners */
            font-size: 16px;
            outline: none; /* Remove outline on focus */
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); /* Subtle shadow for depth */
        }
        button {
            padding: 15px 20px;
            background-color: #007bff; /* Button color */
            color: white;
            border: none;
            border-radius: 0 5px 5px 0; /* Rounded right corners */
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s; /* Smooth transition */
        }
        button:hover {
            background-color: #0056b3; /* Darker shade on hover */
        }
        #results {
            margin-top: 20px;
            padding: 15px;
            background-color: #333; /* Dark gray background */
            border-radius: 5px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        .summary {
            width: 100%;
        }
        .details-container {
            display: flex;
            margin-top: 10px;
            flex-wrap: nowrap;
        }
        .poster {
            flex: 0 0 auto; /* Maintain size */
            margin-right: 20px;
        }
        img {
            max-width: 200px; /* Fixed width for poster image */
            height: auto; /* Maintain aspect ratio */
            border-radius: 5px; /* Rounded corners for images */
        }
        .details {
            flex: 1; /* Fill remaining space */
            font-size: 14px;
            color: #eee; /* Light gray text */
        }
        .details-half {
            padding: 10px;
            box-sizing: border-box; /* Include padding in width */
        }
        @media (max-width: 600px) {
            .details-container {
                flex-direction: column; /* Stack details */
            }
            .poster {
                margin-right: 0; /* Remove margin on small screens */
                margin-bottom: 10px; /* Space below poster */
            }
            .details-half {
                flex: 1 1 100%; /* Full width on mobile */
            }
        }
        #videoFrame {
            display: none; /* Hide iframe initially */
            width: 100%;
            height: 616px;
            border: 1px solid #ccc; /* Add a border */
            margin-top: 20px; /* Space above iframe */
            border-radius: 5px; /* Rounded corners */
        }
    </style>
</head>
<body>
    <h1>Search for Movie Ratings</h1>
    <p>To ensure accurate results, please use the exact IMDb name of the movie or series you want to watch. Enjoy your streaming experience!</p>
    
   <div class="search-container">
        <input type="text" id="movieInput" placeholder="Enter movie title..." required>
        <button id="searchBtn">Search</button>
    </div>
    
   <div id="videoContainer">
        <iframe id="videoFrame" src="" scrolling="no" allow="autoplay" allowfullscreen></iframe>
    </div>
    
   <div id="results"></div>

   <script>
        document.getElementById('searchBtn').addEventListener('click', function() {
            const movieTitle = document.getElementById('movieInput').value.trim();
            if (!movieTitle) {
                alert("Please enter a movie title.");
                return;
            }
            const apiKey = 'd547b285'; // Replace with your OMDb API key
            const url = `https://www.omdbapi.com/?t=${encodeURIComponent(movieTitle)}&apikey=${apiKey}`;

            document.getElementById('results').innerHTML = `Loading...`;
            document.getElementById('videoFrame').style.display = 'none'; // Hide iframe

            fetch(url)
                .then(response => response.json())
                .then(data => {
                    const resultsDiv = document.getElementById('results');

                    if (data.Response === "True") {
                        resultsDiv.innerHTML = `
                            <div class="summary">
                                <h2>${data.Title} (${data.Year}) Streaming</h2>
                                <p>${data.Plot}</p>
                            </div>
                            <div class="details-container">
                                <div class="poster">
                                    <img src="${data.Poster}" alt="${data.Title} poster">
                                </div>
                                <div class="details">
                                    <div class="details-half">
                                        <p><strong>Rated:</strong> ${data.Rated}</p>
                                        <p><strong>Released:</strong> ${data.Released}</p>
                                        <p><strong>Runtime:</strong> ${data.Runtime}</p>
                                        <p><strong>Genre:</strong> ${data.Genre}</p>
                                        <p><strong>Director:</strong> ${data.Director}</p>
                                        <p><strong>Writer:</strong> ${data.Writer}</p>
                                    </div>
                                    <div class="details-half">
                                        <p><strong>Actors:</strong> ${data.Actors}</p>
                                        <p><strong>Language:</strong> ${data.Language}</p>
                                        <p><strong>Country:</strong> ${data.Country}</p>
                                        <p><strong>Awards:</strong> ${data.Awards}</p>
                                        <p><strong>BoxOffice:</strong> ${data.BoxOffice}</p>
                                        <p><strong>IMDb Rating:</strong> ${data.imdbRating}</p>
                                    </div>
                                </div>
                            </div>
                        `;

                        const imdbId = data.imdbID;
                        document.getElementById('videoFrame').src = `https://www.2embed.cc/embed/${imdbId}?autoplay=1&mute=1&cc_load_policy=1`;
                        document.getElementById('videoFrame').style.display = 'block'; // Show iframe
                    } else {
                        resultsDiv.innerHTML = `<p>Movie not found. Please try another title.</p>`;
                        document.getElementById('videoFrame').style.display = 'none'; // Hide iframe
                    }
                })
                .catch(error => {
                    console.error('Error fetching data:', error);
                    document.getElementById('results').innerHTML = `<p>An error occurred while fetching movie data. Please try again later.</p>`;
                    document.getElementById('videoFrame').style.display = 'none'; // Hide iframe
                });
        });
    </script>
</body>
</html>
