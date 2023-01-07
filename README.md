# Made a Movie app in Vanilla Javascript

how to make a Movie App in HTML, CSS, and Vanilla Javascript. 
It will use data from The Movie Database (TMDB) and display it on the user's screen. When you hover over a movie, you will see the details. You can also see the amount of votes of each movie. The app will include a search bar so the user can search for any movie. If you would like more fun projects, please follow me.
Let's get started!


# First we are going to create the basic user interface with HTML:
<!DOCTYPE html>
<html>
  <head>
    <title>Movie App</title>
    <meta charset="UTF-8" />
    <!--linking the stylesheet-->
    <link rel="stylesheet" href="style.css">
  </head>

  <body>
    <header>
      <form id="form">
        <input type="text" id="search" placeholder="Search" class="search" />
      </form>
    </header>
    <main id="main"></main>
    <!--linking the javascript file-->
    <script src="script.js"></script>
  </body>
  
  
# Now let's use CSS to make the app look better:
body {
  font-family: sans-serif;
  background-color: #22254b;
}

header {
  background-color: #373b69;
  display: flex;
  justify-content: flex-end;
  padding: 1rem;
}

.search {
  background-color: transparent;
  border: 2px solid #22254b;
  border-radius: 50px;
  color: #fff;
  font-size: 1rem;
  padding: 0.5rem 1rem;
}

.search::placeholder {
  color: #7378c5;
}

.search::focus {
  background-color: #22254b;
}

main {
  display: flex;
  flex-wrap: wrap;
}

.movie {
  background-color: #373b69;
  border-radius: 3px;
  box-shadow: 0 4px 5px;
  overflow: hidden;
  position: relative;
  margin: 1rem;
  width: 300px;
}
.movie img {
  width: 100%;
}

.movie-info {
  color: #eee;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 1rem 1rem;
  letter-spacing: 0.5px;
}
.movie-info span {
  background-color: #22254b;
  border-radius: 3px;
  font-weight: bold;
  padding: 0.25rem 0.5rem;
}

.movie-info span.green {
  color: green;
}

.movie-info span.orange {
  color: orange;
}

movie-info span.red {
  color: red;
}

.overview {
  background-color: #fff;
  padding: 2rem;
  position: absolute;
  max-height: 100%;
  overflow: auto;
  left: 0;
  bottom: 0;
  right: 0;
  transform: translateY(101%);
  transition: transform 0.3s ease-in;
}
.overview h3 {
  margin-top: 0;
}

.movie:hover .overview {
  transform: translateY(0);
  
# Next let's add Javascript.

 Adding your API key from TMDB - you will need to signup to TMDB for the API key:
        const APIURL =
  "https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=YOUR_API_KEY&page=1";
const IMGPATH = "https://image.tmdb.org/t/p/w1280";
const SEARCHAPI =
  "https://api.themoviedb.org/3/search/movie?&api_key=YOUR_API_KEY&query=";
  
# Remember to replace YOUR_API_KEY with your api key.
Next let's add data display:

const main = document.getElementById("main");
const form = document.getElementById("form");
const search = document.getElementById("search");

const getMovies = async (url) => {
  const resp = await fetch(url);
  const respData = await resp.json();

  console.log(respData);

  showMovies(respData.results);
};

const showMovies = (movies) => {
  main.innerHTML = "";

  movies.forEach((movie) => {
    const { poster_path, title, vote_average, overview } = movie;

    const movieEl = document.createElement("div");
    movieEl.classList.add("movie");

    movieEl.innerHTML = `
      <img
        src="${IMGPATH + poster_path}"
        alt="${title}"
      />
      <div class="movie-info">
        <h3>${title}</h3>
        <span class="${getClassByRate(vote_average)}">${vote_average}</span>
      </div>
      <div class="overview">
        <h3>Overview:</h3>
        ${overview}
      <div>`;
    main.appendChild(movieEl);
  });
};
Then, let's add the Votes functionality:
getMovies(APIURL);

const getClassByRate = (vote) => {
  if (vote >= 8) {
    return "green";
  } else if (vote >= 5) {
    return "orange";
  } else {
    return "red";
  }
};

# Lastly, the search bar functionality:
form.addEventListener("submit", (e) => {
  e.preventDefault();
  const searchTerm = search.value;

  if (searchTerm) {
    getMovies(SEARCHAPI + searchTerm);
    search.value = "";
  }
});
That's it! You have now made a good-looking and working Movie App.
