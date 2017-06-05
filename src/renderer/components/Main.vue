<template>
    <div>
        <md-toolbar>
            <h1 class="md-title">EPEX 2017</h1>
        </md-toolbar>

        <div class="main-content">
            <h2 class="team-name">
             {{ team }}   
            </h2>

            <div class="title">
                <h3 v-if='mid == 0'>Presentation</h3>
                <h3 class="q-a" v-if='mid == 1'>Question & Answer</h3>
                <h3 class="q-t" v-if='mid == -1'>Time Up !!!</h3>
            </div>
            <div class="clock"></div>

            <div class="form">

                <form novalidate @submit.prevent='start'>
                    <md-input-container>
                        <label for="team">Team</label>
                        <md-input type="text" v-model="team" @keyup.enter='start'></md-input>

                    </md-input-container>


                  <md-input-container>
                    <label>Minutes</label>
                    <md-input type="number" v-model="time" @keyup.enter='start' min='0'></md-input>
                </md-input-container>


                <div>              
                    <md-button type='submit' class="md-raised md-primary">Start</md-button>
                    <md-button class="md-raised md-warn" v-on:click.native="stopTimer">Stop</md-button>
                </div>
                </form>
            </div>

            <footer>
                <p class="md-warn">Powered By SCOOLNETWORK</p>
            </footer>
        </div>
    </div>
</template>

<style>
    footer {
        position: fixed;
        bottom: 1px;
        right: 40%;

        text-transform: uppercase;
        color: #00B8D4;
        font-weight: bold;
        font-size: 2em;
    }
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
        font-size: 4em;
        position: absolute;
        left: 37%;
        top: 16%;
    }
    .title {
        position: absolute;
        font-size: 1.5em;
        left: 42%;
        top: 28%;
    }
    .title h3 {
        color: #4CAF50;
    }
    .title .q-a {
        margin-left: -22%;
        color: #FDD835;
    }
    .title .q-t { /*time up*/
        color: #F44336;
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

                        let mid = self.time * 60 / 2;

                        if(mid == time) {
                            self.beep.play();
                            self.middle();
                        }
		        		
		        		if(time == 3) {
                            let i = 0;
                            let interval = setInterval(() => {
                                if(i > 50 || self.stop == true) {
                                    clearInterval(interval);
                                }                             
			        		    self.buzz.play();
                                i++;
                            }, 900);
                            self.timeup();
			        	}
		        	}
		        }

            });
        },
        methods: {
            start() {
                this.clock.setTime(this.time * 60);
                this.clock.start();
                this.stop = false;
                this.mid = 0;
            },
            stopTimer() {            
                this.stop = true;
                this.clock.stop();
                this.buzz.pause();
            },
            middle() {
                this.mid = 1;
            },
            timeup() {
                this.mid = -1;
            }
        },
        data() {
            return {
                time: 0,
                clock: {},
                team: 'EPEX1730',
                buzz: new Audio('static/buzz.mp3'),
                beep: new Audio('static/beep.mp3'),
                stop: true,
                mid: 0
            }
        },
        watch: {
            time(val) {
                this.clock.stop();
                if(val < 0)
                    return;
                this.clock.setTime(val * 60);
            }
        }

    };
</script>