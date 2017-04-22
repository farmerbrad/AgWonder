## AgWonder

Wonder how much feed/water is there?
<img src="https://api-m2x.att.com/v2/charts/940df2cd41b96ea7adf899be034e2234.png?width=800&height=600&type=values">

<h4 class="chart1-title"></h4>
  <canvas id="myChart" width="100" height="300"></canvas>
  
  <section>
    <h1>About</h1>
    <h2>
    "Wonder how much feed/water is in the livestock container?"</h2>
    <p>The AgWonder detects the percent of remaining grain/water in a particular container.</p>
  </section>
</div>
<script src="/js/vendor/Chart.min.js"></script>
<script>

var mode = 0;
var min = 200;
var max = 300;
var percent = 0;
localStorage.setItem("distance", 0);
localStorage.setItem("percent", 0);
(function pollData() {
$.getJSON('https://api-m2x.att.com/v2/devices/f870b9b591619e9cbd2a58ed7cde7d90/streams/', function (data) {
if (data) {
  var storeddistance = localStorage.getItem("distance");

  var distance = data.streams[0].value;
  var deviceId = data.streams[1].value;
  
  var percent = ((max - distance) / (max - min)) * 100;
  
  if (localStorage.getItem("distance") == 0 || storeddistance != distance){
    localStorage.setItem("distance", distance);
    localStorage.setItem("percent", percent);
  }
  $('#displayData').html(distance, deviceId);
  $('.chart-title1').html(+deviceId+ " <i class='fa fa-tint' aria-hidden='true'></i>");
  if(mode == 0){
    console.log('get water level');
  }else{
    console.log('get feed level');
  }
  if(distance != storeddistance){
    buildChart(localStorage.getItem("percent"));
  }
}
setTimeout(pollData, 1000);
});
}());

var ctx = document.getElementById("myChart");

function buildChart(data){
var myChart = new Chart(ctx, {
  type: 'bar',
  data: {
      labels: ["Chicken Water #1"],
      datasets: [{
        label: "Percent Full",
          data: [data],
          backgroundColor: [
              'rgba(255, 99, 132, 0.2)',
              'rgba(54, 162, 235, 0.2)',
              'rgba(255, 206, 86, 0.2)',
              'rgba(75, 192, 192, 0.2)',
              'rgba(153, 102, 255, 0.2)',
              'rgba(255, 159, 64, 0.2)'
          ],
          borderColor: [
              'rgba(255,99,132,1)',
              'rgba(54, 162, 235, 1)',
              'rgba(255, 206, 86, 1)',
              'rgba(75, 192, 192, 1)',
              'rgba(153, 102, 255, 1)',
              'rgba(255, 159, 64, 1)'
          ],
          borderWidth: 1
      }]
  },
  options: {
      scales: {
          yAxes: [{
            display: true,
                      ticks: {
                        beginAtZero: true,
                max: 100
            }
          }]
      },
      max: 100
  }
});
}

</script>

