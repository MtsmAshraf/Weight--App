<template>
    <div>
        <canvas id="myChart" width="400" height="400"></canvas>
    </div>
</template>

<script>
    import Chart from 'chart.js/auto';
    var data = [];
    var labels = [];
    export default {
        name:"chartComponent",
        data(){
            return{
                dataObject:{},
            }
        },
        mounted(){
            if(window.localStorage.getItem("dataObject")){
                this.dataObject = JSON.parse(window.localStorage.getItem("dataObject"));
            }
            if(Object.keys(this.dataObject).length >= 20){
                for(let i = 20; i > 0;i--){
                    if(this.dataObject[(Object.keys(this.dataObject).length) - (i)].weightInKgs != 0 && this.dataObject[(Object.keys(this.dataObject).length) - (i)].weightInKgs != null){
                        data.push(this.dataObject[(Object.keys(this.dataObject).length) - (i)].weightInKgs)
                        labels.push(`${this.dataObject[(Object.keys(this.dataObject).length) - (i)].dayOfTheMonthIndex}/${this.dataObject[(Object.keys(this.dataObject).length) - (i)].monthIndex}`)
                    }
                    
                }
            }else{
                for(let i = Object.keys(this.dataObject).length; i > 0;i--){
                    data.push(this.dataObject[(Object.keys(this.dataObject).length) - (i)].weightInKgs)
                    labels.push(`${this.dataObject[(Object.keys(this.dataObject).length) - (i)].dayOfTheMonthIndex}/${this.dataObject[(Object.keys(this.dataObject).length) - (i)].monthIndex}`)
                }
            }
            
            console.log(this.dataObject);
            console.log(data);
            const ctx = document.getElementById('myChart');
            const myChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Weight in Kgs',
                        data: data,
                        backgroundColor: [
                            `#320021`,
                            `#320021`,
                            `#320021`,
                            `#320021`,
                            `#320021`,
                            `#320021`,
                            // 'rgba(255, 99, 132, 0.2)',
                            // 'rgba(54, 162, 235, 0.2)',
                            // 'rgba(255, 206, 86, 0.2)',
                            // 'rgba(75, 192, 192, 0.2)',
                            // 'rgba(153, 102, 255, 0.2)',
                            // 'rgba(255, 159, 64, 0.2)'
                        ],
                        borderColor: [
                            `rgb(255, 37, 37)`,
                            `rgb(255, 37, 37)`,
                            `rgb(255, 37, 37)`,
                            `rgb(255, 37, 37)`,
                            `rgb(255, 37, 37)`,
                            `rgb(255, 37, 37)`,
                            // 'rgba(255, 99, 132, 1)',
                            // 'rgba(54, 162, 235, 1)',
                            // 'rgba(255, 206, 86, 1)',
                            // 'rgba(75, 192, 192, 1)',
                            // 'rgba(153, 102, 255, 1)',
                            // 'rgba(255, 159, 64, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });
            myChart;
        },
    }
</script>

<style scoped>
</style>