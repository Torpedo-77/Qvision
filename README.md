<!DOCTYPE html>
<html lang='es'>
<head>
  <meta charset='UTF-8'>
  <meta name='viewport' content='width=device-width, initial-scale=1.0'>
  <title>Una película de Minecraft</title>
  <style>
    /* Estilos para la plantilla final PELÍCULAS */
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #1c1c1c;
      color: white;
      text-align: center;
    }
    .container {
      max-width: 600px;
      margin: 0 auto;
      padding: 10px;
      text-align: left;
    }
    .background-image {
      width: 100%;
      height: 250px;
      background-size: cover;
      background-position: center;
      border-radius: 5px;
      margin-bottom: 10px;
    }
    #video-player {
      width: 100%;
      height: 250px;
      display: none;
      border-radius: 5px;
      margin-bottom: 10px;
      z-index: 5;
    }
    .poster {
      width: 120px;
      height: 180px;
      border-radius: 5px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.5);
      position: relative;
      margin-left: 15px;
      margin-top: -140px;
      margin-bottom: 10px;
      z-index: 2;
    }
    #favorite-button {
         position: absolute;
         z-index: 3;
         background-color: #cc0033;
         color: white;
         border: 1px solid #555;
         padding: 5px 10px;
         cursor: pointer;
         font-size: 14px;
         border-radius: 5px;
         margin-left: -40px;
         margin-top: -140px;
         clear: both;
    }
    .chapter-menu {
      display: flex;
      overflow-x: auto;
      gap: 8px;
      padding: 0 0 10px 0;
      scrollbar-width: none;
      clear: both;
      margin-left: 15px;
    }
    .chapter-menu::-webkit-scrollbar {
      display: none;
    }
    .chapter-button {
      background-color: #333;
      color: white;
      border: 1px solid #cc0033;
      padding: 6px 10px;
      font-size: 12px;
      border-radius: 5px;
      cursor: pointer;
      transition: 0.2s;
      white-space: nowrap;
    }
    .chapter-button:hover {
      background-color: #cc0033;
    }
    .active-chapter {
      background-color: #a30026 !important;
      border: 2px solid #ff4d6a;
    }
    .content {
      margin-top: 10px;
    }
    .title {
      font-size: 20px;
      font-weight: bold;
      margin-top: 10px;
      margin-bottom: 0;
      padding: 0 10px;
    }
    .type-line {
      font-size: 14px;
      color: #999;
      margin-top: 5px;
      margin-bottom: 10px;
      padding: 0 10px;
    }
    .synopsis {
      font-size: 14px;
      color: #bdbdbd;
      margin-top: 10px;
      margin-bottom: 10px;
      padding: 0 10px;
      clear: both;
    }
    select {
      background-color: #cc0033;
      color: white;
      border: none;
      padding: 7px;
      font-size: 14px;
      border-radius: 5px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div class='container'>
    <div class='background-image' id='background' style='background-image: url("https://image.tmdb.org/t/p/original/2Nti3gYAX513wvhp8IiLL6ZDyOm.jpg");'></div>
    <img class='poster' id='poster' src='https://image.tmdb.org/t/p/w500/rZYYmjgyF5UP1AVsvhzzDOFLCwG.jpg' alt='Poster'>
    <iframe id='video-player' src='' frameborder='0' allowfullscreen></iframe>
    <button id='favorite-button' class='button' onclick='toggleFavorite()'>🤍</button>
    <div class='content'>
      <div class='title' id='title'>Una película de Minecraft</div>
      <div class='type-line' id='typeLine'>Tipo: Película | Netflix</div>
      <div class='chapter-menu' id='chapter-menu'>
        <button class='chapter-button' onclick='changeChapter("", this)'>Ver ahora</button>
      </div>
      <div class='synopsis' id='synopsis'>Cuatro inadaptados se encuentran luchando con problemas ordinarios cuando de repente se ven arrastrados a través de un misterioso portal al Mundo Exterior: un extraño país de las maravillas cúbico que se nutre de la imaginación. Para volver a casa, tendrán que dominar este mundo mientras se embarcan en una búsqueda mágica con un inesperado experto artesano, Steve.</div>
    </div>
  </div>
  <script>
    const movie = { id: '950387', title: 'Una película de Minecraft', imageUrl: 'https://image.tmdb.org/t/p/w500/rZYYmjgyF5UP1AVsvhzzDOFLCwG.jpg', link: '' };
    const seasons = {};
    const posters = {};
    let currentSeason = 1;
    function changeSeason(season) {
      season = parseInt(season, 10);
      currentSeason = season;
      document.getElementById('title').textContent = movie.title + ' - ' + seasons[season].name;
      document.getElementById('synopsis').textContent = seasons[season].synopsis;
      document.getElementById('poster').src = posters[season];
      const chapterMenu = document.getElementById('chapter-menu');
      chapterMenu.innerHTML = '';
      seasons[season].chapters.forEach(function(ep, index) {
        const btn = document.createElement('button');
        btn.textContent = ep.type + ' ' + (index + 1);
        btn.className = 'chapter-button';
        btn.onclick = function() { changeChapter(ep.url, btn); };
        chapterMenu.appendChild(btn);
      });
      document.getElementById('video-player').style.display = 'none';
      document.getElementById('background').style.display = 'block';
      document.getElementById('poster').style.display = 'block';
      document.getElementById('favorite-button').style.display = 'block';
      // Restablece el margen de .buttons a su valor original
      var btns = document.getElementById('season-selector');
      if(btns && btns.parentElement) { btns.parentElement.style.marginTop = '-30px'; }
    }
    function changeChapter(url, selectedButton) {
      document.getElementById('video-player').src = url;
      document.getElementById('video-player').style.display = 'block';
      document.getElementById('background').style.display = 'none';
      document.getElementById('poster').style.display = 'none';
      document.getElementById('favorite-button').style.display = 'none';
      // Ajusta el margen de .buttons para dejar 15px de separación respecto al reproductor
      var btns = document.getElementById('season-selector');
      if(btns && btns.parentElement) { btns.parentElement.style.marginTop = '15px'; }
      var allButtons = document.querySelectorAll('.chapter-button');
      allButtons.forEach(function(btn) { btn.classList.remove('active-chapter'); });
      if (selectedButton) {
        selectedButton.classList.add('active-chapter');
      }
    }
    function toggleFavorite() {
      let favorites = JSON.parse(localStorage.getItem('favorites')) || [];
      const favBtn = document.getElementById('favorite-button');
      const isFav = favorites.some(function(f) { return f.id === movie.id; });
      if(isFav) {
        favorites = favorites.filter(function(f) { return f.id !== movie.id; });
        favBtn.textContent = '🤍';
        alert('Removido de favoritos');
      } else {
        favorites.push(movie);
        favBtn.textContent = '💓';
        alert('Agregado a favoritos');
      }
      localStorage.setItem('favorites', JSON.stringify(favorites));
    }
    // Película: no se requiere cargar temporada al inicio
  </script>
</body>
</html>
