<template>
    <div class="content-center items-center mx-auto px-4 py-5 grow">
        <div class="flex-row justify-center px-4 py-5">
            <header class="shadow rounded w-full focus:outline-none focus:shadow-outline px-4 py-4 text-center">
                <p class="text-3xl py-2">Cálculo das raízes de uma equação do 2° grau</p>
            </header>
        </div>
        <div class="flex-row justify-center px-4 py-5">
            <div class="m-2">
                <label class="block text-xl text-gray-700 font-bold mb-2" for="a">
                    Coeficiente a -> <span id="coef_a">{{ coef_a }}x²</span>
                </label>
                <input
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="a" type="text" pattern="[0-9]+" v-model="coef_a" @focus="$event.target.select()">
            </div>
            <div class="m-2">
                <label class="block text-xl text-gray-700 font-bold mb-2" for="b">
                    Coeficiente b -> <span id="coef_b">{{ coef_b }}x</span>
                </label>
                <input
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="b" type="text" pattern="[0-9]+" v-model="coef_b" @focus="$event.target.select()">
            </div>
            <div class="m-2">
                <label class="block text-xl text-gray-700 font-bold mb-2" for="c">
                    Coeficiente c -> <span id="coef_c">{{ coef_c }}</span>
                </label>
                <input
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="c" type="text" pattern="[0-9]+" v-model="coef_c" @focus="$event.target.select()">
            </div>
        </div>
        <div class="flex-row justify-center px-4 py-5">
            <p class="shadow rounded text-2xl w-full focus:outline-none focus:shadow-outline px-4 py-4 text-center"
                v-if="(coef_a || coef_b || coef_c) != null">
                <b>Equação:</b> {{ coef_a }}x² <span v-if="coef_b && coef_b > 0"> + {{ coef_b }}x </span> <span
                    v-else-if="coef_b && coef_b < 0"> - {{ Math.abs(coef_b) }}x </span> <span v-else-if="coef_b"> + {{
                        coef_b }}x</span> <span v-if="coef_c && coef_c > 0"> +
                    {{ coef_c }}</span> <span v-else-if="coef_c && coef_c < 0"> - {{ Math.abs(coef_c) }} </span> <span
                    v-else-if="coef_c"> + {{ coef_c }}</span>
                = 0
            </p>
        </div>
        <div class="flex-row justify-center px-4 py-5" v-if="coef_a && coef_b">
            <div
                class="flex-row justify-center shadow rounded text-2xl focus:outline-none focus:shadow-outline px-4 py-4 text-center">
                <div class="pb-2" v-text="resposta"></div>
                <div id="x1">x1 = {{ equacao2Grau(coef_a, coef_b, coef_c)[0] }}</div>
                <!-- Return value from position in array -->
                <div id="x2">x2 = {{ equacao2Grau(coef_a, coef_b, coef_c)[1] }}</div>
                <!-- Return value from position in array -->
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'Raízes',
    data() {
        return {
            coef_a: null,
            coef_b: null,
            coef_c: null,
            resposta: ''
        }
    },
    methods: {
        equacao2Grau(a, b, c) {
            let delta, x1, x2, answer, reply

            answer = Array() // or answer = []

            delta = (b * b - 4 * a * c)


            if (delta >= 0) {
                x1 = (-b + Math.sqrt(delta)) / (2 * a)
                x2 = (-b - Math.sqrt(delta)) / (2 * a)

                if (x1 === x2) {
                    answer = answer.concat(x1, x2)
                    this.resposta = 'Raízes: A equação possui duas raízes reais e iguais'
                    return answer
                }
                else {
                    answer = answer.concat(x1, x2)
                    this.resposta = 'Raízes: A equação possui duas raízes reais e diferentes'
                    return answer
                }

            }
            else if (delta < 0) {
                let aux = -b / (2 * a)  // Parte real
                let expr = Math.sqrt(-delta) / (2 * a)  // Parte complexa
                // Construção do número complexo em forma de string
                let x1 = aux.toString().concat(' + ', expr, 'i')
                let x2 = aux.toString().concat(' - ', expr, 'i')
                answer = answer.concat(x1, x2)

                this.resposta = 'Raízes: A equação possui duas raízes imaginárias'
                return answer
            }
            else {
                this.resposta = 'Raízes:'
                return answer
            }
        },
    },
}
</script>