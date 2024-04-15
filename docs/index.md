<div id="container">
<div id="greeting">📖  Hello，世界！</div>
<div>
<div>
<div>
<div>
<div>
<div>
<div id="time"></div>

<script>

function updateTime() {

    var now = new Date();

    var year = now.getFullYear();
    var month = now.getMonth() + 1;
    var day = now.getDate();
        
    var hours = now.getHours();
    var minutes = now.getMinutes();
    var seconds = now.getSeconds();

    // Add leading zeros
    if (month < 10) {
    month = "0" + month;
    }
    if (day < 10) {
    day = "0" + day;
    }

    if (hours < 10) {
        hours = "0" + hours;
    }
    if (minutes < 10) {
        minutes = "0" + minutes;
    }
    if (seconds < 10) {
        seconds = "0" + seconds;
    }

    // Update time
    document.getElementById("time").innerHTML =  year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds;

}

    // Update the time every second
    setInterval(updateTime, 1000);

</script>

<style>

    #container {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }

    #greeting {
        font-size: 100px;
        background: linear-gradient(to right, red, orange, yellow, green, blue, indigo, violet);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
    }

    #time {
        font-size: 64px;
        /* background: linear-gradient(to right, red, orange, yellow, green, blue, indigo, violet); */
        background: linear-gradient(to right, violet, indigo, blue, green, yellow, orange, red);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
    }

</style>
