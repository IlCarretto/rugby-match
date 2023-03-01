<script>
export default {
    name: "AppMain",
    data() {
        return {
            matchesInfo: [],
            myCard: ""
        }
    },
    methods: {
        getMatchesCards() {
            fetch('matches.json')
            .then(resp => resp.json())
            .then(resp => {
                this.matchesInfo = resp.data;
            })
        },
        getImagePath: function (imgPath) {
            return new URL(imgPath, import.meta.url).href;
        },
        getOrdinal(number) {
            if (number % 100 >= 11 && number % 100 <= 13) {
            return number + 'th';
            } else if (number % 10 === 1) {
            return number + 'st';
            } else if (number % 10 === 2) {
            return number + 'nd';
            } else if (number % 10 === 3) {
            return number + 'rd';
            } else {
            return number + 'th';
            }
        }
    },
    created() {
        this.getMatchesCards();
    }
}
</script>

<template>
    <div class="container">
        <section v-for="matchGroup in this.matchesInfo" class="matches">
            <h3 class="matches-date text-center">{{ matchGroup.date.toUpperCase() }}</h3>
            <ul class="matches-list">
                <li v-for="match in matchGroup.matches">
                    <div class="ms-card position-relative">
                        <div class="ms-card-inner d-flex">
                            <!-- Card Left -->
                            <div :style="{ 'background-color': 'rgb(' + match.teams[0].color + ')'}" class="ms-card-left d-flex position-relative">
                                <img style="" :src="getImagePath(`../assets/${match.teams[0].themeImage}.png`)" alt="">
                            </div>
                            <!-- /Card Left -->
            
                            <!-- Card Text -->
                            <div class="ms-card-text d-flex flex-column position-absolute align-items-center justify-content-between">

                                <!-- Card Text Top -->
                                <div class="ms-card-text-match-type d-flex justify-content-center">
                                    <span>{{match.type.toUpperCase()}}</span>
                                </div>
                                <!-- /Card Text Top -->

                                <!-- Card Text Teams -->
                                <div class="ms-card-teams d-flex justify-content-between align-items-center">
                                    <!-- Team One -->
                                        <div class="team-content position-relative text-center d-flex align-items-center">
                                            <div class="team-one">
                                                <img id="team-first" :src="getImagePath(`../assets/${match.teams[0].logo}`)" alt="">
                                                <div class="team-position d-flex justify-content-center align-items-end">
                                                    <span>(P)</span>
                                                    <!-- Verificare se questo numero termina con: 1 -> st, 2 -> nd, 3 -> rd -->
                                                    <h4>{{getOrdinal(match.teams[0].position)}}</h4>
                                                </div>
                                            </div>
                                        </div>
                                    <div class="team-one-info d-flex justify-content-end align-items-center">
                                        <div class="team-name me-2">
                                            <h1>{{match.teams[0].name}}</h1>
                                        </div>
                                        <div class="team-score left d-flex align-items-center justify-content-center me-2">
                                            {{match.teams[0].score}}
                                        </div>
                                    </div>
                                    <!-- /Team One -->

                                    <!-- Team Two -->
                                    <div class="team-one-info d-flex align-items-center">
                                        <div class="team-score right d-flex align-items-center justify-content-center ms-2">
                                        {{match.teams[1].score}}
                                        </div>
                                        <div class="team-name ms-2">
                                            <h1>{{match.teams[1].name}}</h1>
                                        </div>
                                    </div>
                                    <div class="team-content position-relative text-center">
                                        <img id="team-two" :src="getImagePath(`../assets/${match.teams[1].logo}`)" alt=""> 
                                        <div class="team-position d-flex justify-content-center align-items-end">
                                            <h4>{{getOrdinal(match.teams[1].position)}}</h4>
                                            <span>(P)</span>
                                        </div>
                                    </div>
                                    <!-- /Team Two -->
                                </div>
                                <!-- /Card Text Teams -->

                                <!-- Card Stadium -->
                                <div class="ms-card-stadium text-center mb-3">
                                    <h5>{{match.place.toUpperCase()}}</h5>
                                    <img src="../assets/united-rugby-copy.png" alt="">
                                </div>
                                <!-- /Card Stadium -->
                            </div>      
                            <!-- /Card Text -->

                            <!-- Card Right -->
                            <div :style="{ 'background-color': 'rgb(' + match.teams[1].color + ')'}" class="ms-card-right d-flex position-relative">
                                <img :src="getImagePath(`../assets/${match.teams[1].themeImage}.png`)" alt="">
                            </div>
                            <!-- /Card Right -->
                        </div>
                    </div>
                </li>
            </ul>
        </section>
    </div>
</template>

<style lang="scss" scoped>
@use "../scss/general.scss" as *;
body {
    background-color: #e6e6e6;
}

.matches {
    padding: 2rem;

    .matches-date {
        font-weight: 700;
    }
}
// General Card
.ms-card {
    margin-bottom: 1.6rem;

    // Card-Left
    &-left {
        border-bottom-left-radius: 20px;
        justify-content: flex-end;
    }
    // Card-Right
    &-right {
        border-bottom-right-radius: 20px;
        justify-content: flex-start;
    }

    // Both left and right
    &-left,
    &-right {
        width: 50%;

        img {
            position: relative;
            width: calc(100% - 20px);
            opacity: .5;
        }
    }

    // Card text
    .ms-card-text {
        width: calc(100% - 20px * 2);
        top: 0;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        z-index: 1;
        
        .ms-card-text-match-type {
            color: white;
            background-color: #ff4b5a;
            border-bottom-right-radius: 5px;
            border-bottom-left-radius: 5px;
            padding: .2rem 1rem;
            font-size: .9rem;
            letter-spacing: 0.1em;
            font-weight: 600;
        }
        
        // Card Teams Inner
        .ms-card-teams {
            width: 100%;

            img {
                width: 150px;
            }

            .team-position {
                color: white;
            }
        }
        
        .team-one-info,
        .team-two-info {
            width: 50%;
        }

        .team-name {
            color: white;
        }
        
        .team-score {
            text-align: center;
            width: 75px;
            height: 90px;
            font-size: 2.6rem;
            font-weight: 700;
            background-color: white;

        }
        
        .left {
            border-bottom-left-radius: 10px;
        }

        .right {
            border-bottom-right-radius: 10px;
        }

        .ms-card-stadium {
            color: white;

            img {
                height: 50px;
            }

            h5 {
                letter-spacing: 0.1em;
            }
        }
    }
}
</style>