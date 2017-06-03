<template>
    <div>
        <md-toolbar>
            <h1 class="md-title">EPEX 2017</h1>
        </md-toolbar>

        <div class="main-content">
            <h2 class="team-name">
             {{ team }}   
            </h2>
            <div class="clock"></div>

            <div class="form">

                <form novalidate @submit.prevent='start'>
                    <md-input-container>
                        <label for="team">team</label>
                        <md-input type="text" v-model="team" @keyup.enter='start'></md-input>

                    </md-input-container>


                  <md-input-container>
                    <label>Minutes</label>
                    <md-input type="number" v-model="time" @keyup.enter='start'></md-input>
                </md-input-container>


                <div>              
                    <md-button type='submit' class="md-raised md-primary">Start</md-button>
                    <md-button class="md-raised md-warn" v-on:click.native="stopTimer">Stop</md-button>
                </div>

                </form>
            </div>
        </div>
    </div>
</template>

<style>
    .text-center {
        text-align: center;
    }
    .main-content {
        padding: 1em;
    }
    .clock {
        left: 35%;
        top: 40%;
        position: fixed;
    }
    .team-name {
        text-align: center;
        font-size: 4em;
    }
    .form {
        position: fixed;
        bottom: 1px;
        right: 1px;
        width: 30%;
        padding: 1em;
        border: 1px solid;
    }
</style>
<script>
    export default {
        mounted() {
            let self = this;
            window.clock = this.clock = $('.clock').FlipClock({
                autostart: false,
                countdown: true,
                clockFace: 'MinuteCounter',
                callbacks: {
		        	interval() {
		        		let time = this.factory.getTime().time;
		        		
		        		if(time == 3) {
                            let i = 0;
                            let interval = setInterval(() => {
                                if(i > 50 || self.stop == true) {
                                    clearInterval(interval);
                                    console.log('done')
                                }                             
			        		    self.buzz.play();
                                i++;
                            }, 900);
			        	}
		        	}
		        }

            });
        },
        methods: {
            start() {
                this.clock.start();
                this.stop = false;
            },
            stopTimer() {            
                this.stop = true;
                this.clock.stop();
                this.buzz.pause();
            }
        },
        data() {
            return {
                time: 0,
                clock: {},
                team: 'EPEX1730',
                buzz: new Audio('static/buzz.mp3'),
                stop: true
            }
        },
        watch: {
            time(val) {
                this.clock.stop();
                this.clock.setTime(val);
            }
        }

    };
</script>