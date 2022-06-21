
# Netflix-Clone

1. This explains the code I used in Netflix clone
2. All the directories and files inside netflic-main folder that I have created inside src directory. 
3. create dotenv file in root directory and add your API 

## Author

- [Manish Yadav](#)

## ⮚	Netflix.js

```javascript
import React from 'react'
import Row from './components/Row';
import Header from './components/Header';
import Banner from './components/Banner';
import Footer from './components/Footer.js';
import './components/main.css';
import requests from './url-access/requests.js';

const Netflix = () => {
    return (
    <>
      <div className="BodyStyle">
        <Header />
        <Banner fetchUrl={requests.fetchTrending} />
        <Row title="Netflix Original" fetchUrl={requests.fetchNetflixOriginals} isLargeRow />
        <Row title="Trending" fetchUrl={requests.fetchTrending} />
        <Row title="Top Rated" fetchUrl={requests.fetchTopRated} />
        <Row title="Action Movies" fetchUrl={requests.fetchActionMovies} />
        <Row title="Horror Movies" fetchUrl={requests.fetchHorrorMovies} />
        <Row title="Romance Movies" fetchUrl={requests.fetchRomanceMovies} />
        <Row title="Documentary Movies" fetchUrl={requests.fetchDocumentariesMovies} />
        <Row title="Comedy Movie" fetchUrl={requests.fetchComedyMovies} />
        <Footer />
      </div>

    </>
  )
}
export default Netflix

```

### ⮚ .env
``` env
REACT_APP_API_KEY = 16d9855ebddaf971ecca792b28c08eed
```


## ⮚	url-acces>request.js

```javascript
const API_KEY = process.env.REACT_APP_API_KEY
const requests = {
    fetchTrending: `/trending/all/week?api_key=${API_KEY}&language=en-US`,
    fetchNetflixOriginals: `/discover/tv?api_key=${API_KEY}&with_networks=213`,
    fetchTopRated: `/movie/top_rated?api_key=${API_KEY}&language=en-US`,
    fetchActionMovies: `/discover/movie?api_key=${API_KEY}&with_genres=28`,
    fetchComedyMovies: `/discover/movie?api_key=${API_KEY}&with_genres=35`,
    fetchHorrorMovies: `/discover/movie?api_key=${API_KEY}&with_genres=27`,
    fetchRomanceMovies: `/discover/movie?api_key=${API_KEY}&with_genres=10749`,
    fetchDocumentariesMovies: `/discover/movie?api_key=${API_KEY}&with_genres=99`  
}
export default requests;

```

## ⮚	url-acces>axios.js

```javascript
import axios from "axios";
const instance = axios.create({
    baseURL: "https://api.themoviedb.org/3",
});
export default instance;

```

## ⮚	components>Header.js

```javascript
import React, {useState, useEffect} from 'react'
import NetflixLogo from "../media/NetflixLogo.png"

const Header = () => {
  const [show, handleShow] = useState(false);

  useEffect(() => {
    window.addEventListener("scroll",()=>{
      if (window.scrollY>100){
          handleShow(true);
      } else handleShow(false);
    });
    return ()=>{
      // window.removeEventlistener("scroll");
      console.log("removedEventListner")
    };
  }, []);

  return (
    <>
    <div className={`${show? "navbar-show":"navbar-hidden"}`} >
    <img
      className="navbar-logo"
      src={NetflixLogo}
      alt="Netflix Logo"
    />
    </div>
    </>
  )
}
export default Header


```

## ⮚	components>Banner.js

```javascript
import React, {useState, useEffect} from 'react'
import NetflixLogo from "../media/NetflixLogo.png"

const Header = () => {
  const [show, handleShow] = useState(false);

  useEffect(() => {
    window.addEventListener("scroll",()=>{
      if (window.scrollY>100){
          handleShow(true);
      } else handleShow(false);
    });
    return ()=>{
      // window.removeEventlistener("scroll");
      console.log("removedEventListner")
    };
  }, []);

  return (
    <>
    <div className={`${show? "navbar-show":"navbar-hidden"}`} >
    <img
      className="navbar-logo"
      src={NetflixLogo}
      alt="Netflix Logo"
    />
    </div>
    </>
  )
}
export default Header


```

## ⮚	components>Footer.js

```javascript
import React from 'react'

const Footer = () => {
  return (
    <div>Footer</div>
  )
}

export default Footer

```

## ⮚	components>Row.js

```javascript
import React, { useState, useEffect } from 'react'
import axios from "../url-access/axios";
import './main.css';
import YouTube from 'react-youtube'
import movieTrailer from "movie-trailer"

const Row = (props) => {
  // console.log(props.title);
  // console.log(props.fetchUrl);
  const [movies, setMovies] = useState([]);

  // to fetch the url using axios
  useEffect(() => {
    async function fetchData() {
      const request = await axios.get(props.fetchUrl);
      // console.log(request);
      setMovies(request.data.results)
      return request;
    }
    fetchData();
  }, [props.fetchUrl]);

  // console.log(movies);
  const YoutubeStyle = {
    height: '390',
    width: '640',
    playerVars: { autoplay: 1, }
  };
  const [trailerURL, setTrailerURL] = useState("");
  const PlayTrailer = (movie) => {
    // console.log(movie);
    if (trailerURL) {
      setTrailerURL("");
    } else {
      console.log(movie?.title || movie?.name || movie?.original_name)
      movieTrailer(movie?.title || movie?.name || movie?.original_name)
        .then((url) => {
          // console.log("url", url)
          const urlParams = new URLSearchParams(new URL(url).search);
          setTrailerURL(urlParams.get("v"))
        })
        .catch((error) => console.log(error));
    }
  };
  const EndPlayTrailer = () => {
    if (trailerURL) {
      setTrailerURL("");
    } else {
      console.log("TrailerURL",trailerURL);
    }
  };

  return (
    <>
      <div className="movie-lists" onClick={() => EndPlayTrailer()}>
        <h2>{props.title}</h2>
        <div className="posters-row">
          {movies.map(movie => (
            <img
              key={movie.id}
              onClick={() => PlayTrailer(movie)}
              className={`image-in-row ${props.isLargeRow && "image-in-row-large"}`}
              src={`https://image.tmdb.org/t/p/w200${props.isLargeRow ? movie.poster_path : movie.backdrop_path}`}
              alt={movie.name}
            />
          ))}
        </div>
        <div className="movie-player">
          {trailerURL && <YouTube videoId={trailerURL} opts={YoutubeStyle} />}
        </div>

      </div>
    </>
  )
}

export default Row


```


## ⮚	main.css

```css

.BodyStyle{
    background-color:#1a1818;
    color:white;
}



.movie-lists{
    margin:20px;
}
/* Scroll Bar */
.posters-row{
    display: flex;
    overflow-y: hidden;
    overflow-x: scroll;
}
.posters-row::-webkit-scrollbar{
    display:none;
}
/* Image inside the ScrollBar */
.image-in-row{ 
    object-fit: contain;
    width: 100%; 
    max-height: 200px;
    margin-right: 1px;
    transition: transform 450ms; 
    padding:10px;
}
.image-in-row:hover{
    transform: scale(1.09);
}
    /* To maximize the size of first row */
.image-in-row-large{
    max-height:250px;
}
.image-in-row-large:hover{
    transform: scale(1.1);
}



/* Banner CSS Part */
.banner-outer{
    object-fit:contain;
    height:448px;
    /* background-color:cyan;
    background-size:cover;
    background-image: url("https://image.tmdb.org/t/p/w200/ze4lhw0oLBHYmlM2KuZjBg0Sq6H.jpg");
    background-position:"center center"; */
}
.banner-content{
    margin-left:30px;
    padding-top:140px;
    height:190px;
}
.banner-title{
    font-size:3rem;
    font-weight:800;
    padding-bottom:0.3rem;
}
.banner-details{
    font-size:0.8rem;
    width:45rem;
    line-height:1.3;
    padding-top:1rem;
    background:transparent;
}
@media only screen and (max-width: 600px) {
    .banner-details{
        font-size:0.8rem;
        width:20rem;
        line-height:1.2;
        padding-top:1rem;
    }
}


.banner-buttons{

}
.banner-button{
    cursor: pointer;
    color: #fff;
    outline:none;
    border:none;
    font-weight:700;
    border-radius:0.2rem;
    padding: 0.5rem 1.8rem;
    margin-right:1rem;
    background-color: rgba(51,51,51,0.5);
    text-align:center;
}
.banner-button:hover{
    background-color: rgb(26, 25, 25);
    color: rgba(221, 210, 210, 0.5);
    transition: all 0.5s;
}
.banner-fade-bottom{
    height:7.4rem;
    background-image:linear-gradient(
        180deg,
        transparent,
        rgba(37,37,37,0.61),
        #111
    )
}


/* Navbar CSS Part======================================== */
.navbar-show{
    position:fixed;
    top:0;
    display:flex;
    padding:5px;
    width:100%;
    height:50px;
    background-color:black;
    z-index:1;
    visibility: visible;
    transition-timing-function:ease-in;
    transition: all 0.5s;
}
.navbar-hidden{
    position:fixed;
    top:0;
    display:flex;
    padding:5px;
    width:100%;
    height:50px;
    visibility: hidden;
}
.navbar-logo{
    height:70px;
    position: fixed;
    object-fit:contain;
    visibility: visible;
}
.navbar-profile-logo{
    position:fixed;
    width:30px;
    right:20px;
    object-fit:contain;
}


.movie-player{
    height: '390';
    width: '400';
}

```
