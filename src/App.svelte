<script>
  let src = 'my-app/assets/svelte.png';
  console.log("Getting Locaction...");    
  if (navigator) {
      navigator.geolocation.getCurrentPosition((pos) => {
          console.log(`Current position : ${pos.coords.longitude}\t${pos.coords.latitude}`);
          getWeather(pos);
      })
  } else {
      console.warn("Geolocation not supported in this context");
  }
  
  function getFormattedDate(date) {
      return date.getFullYear()+"-"+(Number(date.getMonth()) + 1) +"-"+date.getDate();
  }

  async function getWeather(pos) {
      const ele = document.querySelector("#result");
      const date = new Date;

      var data = await fetch(`https://api.open-meteo.com/v1/forecast?latitude=${pos.coords.latitude}&longitude=${pos.coords.longitude}&hourly=temperature_2m&start_date=${getFormattedDate(date)}&end_date=${getFormattedDate(date)}`)
          .then((req) => {
              return req.json();
          });
      
      console.log(data);
      
      ele.innerHTML = data.hourly.temperature_2m[date.getHours()] + " C";
          
      }
    
      function showTemp() {
        var x = document.getElementById("text");
        var y = document.getElementById("button");
        if (x.style.display === "block") {
          x.style.display = "none";
        } 
        else {
          x.style.display = "block";
          y.style.display = "none";
          
        }
      }
      
  
</script>
<style> 
  h1.text {
    border: 2cm;
    border-style: solid;
    border-color: teal;
    border-radius: 15px;
    background-color: teal;
    display:none;
  }

  
  #button {
    display:block;
  }

</style>

<div>
  <h1 class = "text" id ="text"> 
      The current temperature is: <div id="result"></div>
  </h1> 
  <button on:click={showTemp} id = "button">
    Click me to show the weather!
  </button>
</div>
 