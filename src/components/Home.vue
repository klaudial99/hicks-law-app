<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-6">
                <div class="row justify-content-center">
                    <div class="col-3 text-start">
                        <label for="select-options" class="form-label">Liczba opcji do wyboru:</label>
                        <select class="form-select" id="select-options" v-model="optionsAmount">
                            <option value="2">2</option>
                            <option value="3">3</option>
                            <option value="4">4</option>
                            <option value="6">6</option>
                            <option value="8">8</option>
                            <option value="10">10</option>
                        </select>
                    </div>
                    <div class="col-3 align-self-end text-end">
                        <button type="button" class="btn btn-outline-dark" @click="startTest">
                            Rozpocznij
                        </button>
                    </div>
                </div>
                <div class="row mt-4 justify-content-center">
                    <div class="col-6 text-start" v-if="this.randomChar !== ''">
                        <label for="instruction" class="form-label">Instrukcja:</label>
                        <p id="instruction" class="p-2">Kliknij przycisk zawierający literę: {{randomChar}}</p>
                    </div>
                </div>
                <div class="row mt-4 justify-content-center">
                    <div class="col-6 text-start" v-if="timeDiff !== 0">
                        <p id="success" class="fw-bolder">Różnica: {{timeDiff}}</p>
                    </div>
                    <div class="col-6 text-start" v-if="error">
                        <p id="error" class="fw-bolder">Wybrano zły przysik!</p>
                    </div>
                </div>
            </div>
            <div class="col-6">
                <div class="row" v-for="char in charsInUse" :key="char" :id="char">
                    <div class="col-3 p-2" >
                        <button class="w-100 char-btn py-2" @click="endTest(char)">
                            {{ char }}
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
    name: 'Home',
    data() {
        return {
            optionsAmount: 2,
            randomChar: '',
            startTime: null,
            endTime: null,
            timeDiff: 0,
            characters: ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'i', 'j'],
            charsInUse: [],
            error: false


        }
    },
    methods: {
        startTest() {
            this.startTime = new Date()
            this.timeDiff = 0
            this.error = false
            this.charsInUse = this.characters.sort(() => Math.random() - 0.5).slice(0, this.optionsAmount)

            const randomIndex = Math.floor(Math.random() * this.charsInUse.length);
            this.randomChar = this.charsInUse[randomIndex]
        },
        endTest(char) {
            if(char === this.randomChar) {
                this.endTime = new Date()
                const diff = this.endTime.getTime() - this.startTime.getTime()
                this.timeDiff = this.msToSec(diff)
                this.error = false
            }
            else {
                this.timeDiff = 0
                this.error = true
            }
        },
        msToSec(ms) {
            const s = ms / 1000
            return s + 's'
        }
    }

}
</script>
<style scoped>
#instruction {
    border: 1px solid dimgrey;
    border-radius: 5px;
}

#success {
    color: darkgreen;
}

#error {
    color: darkred;
}

.char-btn {
    border-radius: 5px;
    color: white;
    background-color: #383838;
    border: 2px solid #383838;
}

.char-btn:hover {
    color: #383838;
    background-color: white;
}
</style>
