<template>
    <div class="col d-flex flex-column align-items-center mx-auto">
        <div class="text-center">
            <img src="/images/riverlane_logo_square.png"> 
            <p class="fs-5 fw-bold">Quantum Circuit Builder</p>
        </div>
        
        <div class="rounded border p-3 bg-light text-center mb-4">
            <p class="fw-bold text-muted">Length</p>
            <input 
                type="number" 
                class="form-control form-control-lg mx-auto text-center fs-2"
                style="width:150px"
                v-model="circuit.length"
                @keydown="validateInput"
                @keyup="getRandomGates"
            >
        </div>
        <div class="d-flex flex-column justify-content-center align-items-center" style="min-height:200px;">
            <div v-if="this.circuit.length > 0 && this.error == false"  class="d-flex">
                <div class="qubit border-end">Qubit</div>
                <div class="d-flex flex-wrap">
                    <div v-for="gate in this.randomGates" :key="gate.index" class="d-flex">
                        <div class="gate-divide">
                            <i class="fal fa-horizontal-rule"></i>
                        </div>
                        <div class="gate shadow">
                            {{gate}}
                        </div>
                    </div>
                </div>
            </div>
            <div v-else-if="this.error == true " class="text-center text-muted">
                <i class="fas fa-exclamation-circle fa-3x"></i><br />
                Maximum length is 99
            </div>

            <div v-else class="text-center text-muted">
                <i class="fas fa-arrow-up fa-3x"></i> <br />
                Please enter a circuit length
            </div>
        </div>
    </div>
</template>

<script>
export default {
   
    
    data(){
        return {
            error : false,
            circuit : {
                length : null,
            },
            gates : ['X', 'Y', 'Z', 'H', 'S', 'R', 'T' ],
            randomGates : []
        }
    },


    methods :{
        validateInput(e){ 
            if(e.key.match(/[0-9]/) == null && e.keyCode != 8 ){ 
                 e.preventDefault()     
            } 
        },

        getRandomGates(){
            this.randomGates = []
            if(this.circuit.length <= 99){
                this.error = false;
                for (let i = 0; i < this.circuit.length; i++){
                    const randomGate = Math.floor(Math.random() * this.gates.length);
                    this.randomGates.push(this.gates[randomGate])
                }
                this.randomGates.push('M')
            } else {
                this.error = true;
            }
        }
    },

    // mounted() {
    //     this.getRandomGates()
    // }
    
}
</script>

<style scoped>

    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
    }

    .qubit {
        display: flex;
        justify-content:center;
        flex-direction: column;
        align-items: center;
        width:80px;
        height:50px;
        margin-left:5px;
        margin-right:5px;
        padding-right:5px;
        background: white;
        font-weight: 500;
        font-size:1.2em;
    }

    .gate-divide {
        display: flex;
        height:50px;
        justify-content:center;
        flex-direction: column;
        align-items: center;
        margin-bottom:10px;
        margin-left:5px;
        margin-right:5px;

    }

    .gate {
        display: flex;
        justify-content:center;
        flex-direction: column;
        align-items: center;
        width:80px;
        height:50px;
        background: #007162;
        margin-bottom:10px;
        color:white;
        font-size:30px;
    }

</style>>