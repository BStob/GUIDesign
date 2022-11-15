
<script>
    let src = "svelte.png";
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
</script>
<style> 

</style>

<div>
    <img src = {src} alt="">
    <h1> 
        
    </h1> 
</div>
   