<template>
    <div class="justify-center items-center mx-auto px-4 py-5 grow min-h-screen">
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
                    id="a" type="text" pattern="[0-9]+" v-model="coef_a" @focus="$event.target.select()"
                    @keypress="isNumber($event)" placeholder="Apenas números inteiros">
            </div>
            <div class="m-2">
                <label class="block text-xl text-gray-700 font-bold mb-2" for="b">
                    Coeficiente b -> <span id="coef_b">{{ coef_b }}x</span>
                </label>
                <input
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="b" type="text" pattern="[0-9]+" v-model="coef_b" @focus="$event.target.select()"
                    @keypress="isNumber($event)" placeholder="Apenas números inteiros">
            </div>
            <div class="m-2">
                <label class="block text-xl text-gray-700 font-bold mb-2" for="c">
                    Coeficiente c -> <span id="coef_c">{{ coef_c }}</span>
                </label>
                <input
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="c" type="text" pattern="[0-9]+" v-model="coef_c" @focus="$event.target.select()"
                    @keypress="isNumber($event)" placeholder="Apenas números inteiros">
            </div>
        </div>
        <div class="flex-row justify-center px-4 py-5">
            <p class="shadow rounded text-2xl w-full focus:outline-none focus:shadow-outline px-4 py-4 text-center"
                v-if="(coef_a || coef_b || coef_c) != null">
                <b>Equação:</b> {{ coef_a }}x²
                <span v-if="coef_b && coef_b > 0"> + {{ coef_b }}x</span>
                <span v-else-if="coef_b && coef_b < 0"> - {{ Math.abs(coef_b) }}x</span>
                <span v-if="coef_c && coef_c > 0"> + {{ coef_c }}</span>
                <span v-else-if="coef_c && coef_c < 0"> - {{ Math.abs(coef_c) }}</span>
                = 0
            </p>
        </div>
        <div class="flex-row justify-center px-4 py-5" v-if="coef_a && coef_b">
            <div
                class="flex-row justify-center shadow rounded text-2xl focus:outline-none focus:shadow-outline px-4 py-4 text-center">
                <div class="pb-2" v-text="resposta"></div>
                <div id="x1">x<sub>1</sub> = {{ equacao2Grau(coef_a, coef_b, coef_c)[0] }}</div>
                <!-- Return value from position in array -->
                <div id="x2">x<sub>2</sub> = {{ equacao2Grau(coef_a, coef_b, coef_c)[1] }}</div>
                <!-- Return value from position in array -->
            </div>
        </div>
        <!-- Modal function window -->
        <div>
            <div class="flex justify-center items-center">
                <button @click="showModal = true" class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
                    v-if="coef_a">
                    Mostrar Gráfico
                </button>
            </div>
            <div v-if="showModal" class="fixed inset-0 bg-gray-800 bg-opacity-50 flex justify-center items-center z-50">
                <div class="bg-white rounded-lg shadow-lg w-11/12 md:w-2/3 lg:w-1/2 p-6 relative">
                    <button @click="showModal = false" class="absolute top-2 right-2 text-gray-500 hover:text-gray-700">
                        ✖
                    </button>
                    <h2 class="text-xl font-bold text-center mb-4">Gráfico da Equação</h2>
                    <p class="text-center mb-3">y = {{ coef_a }}x²
                        <span v-if="coef_b && coef_b > 0"> + {{ coef_b }}x</span>
                        <span v-else-if="coef_b && coef_b < 0"> - {{ Math.abs(coef_b) }}x</span>
                        <span v-if="coef_c && coef_c > 0"> + {{ coef_c }}</span>
                        <span v-else-if="coef_c && coef_c < 0"> - {{ Math.abs(coef_c) }}</span>
                    </p>
                    <p class="text-center mb-3" v-text="resposta"></p>
                    <p class="text-center">x<sub>1</sub> = {{ equacao2Grau(coef_a, coef_b, coef_c)[0] }} ;
                        x<sub>2</sub> = {{ equacao2Grau(coef_a, coef_b, coef_c)[1] }}</p>
                    <div id="plot" class="flex justify-center items-center w-full h-100">
                    </div>
                </div>
            </div>
        </div>
        <footer class="flex-row shadow rounded z-20 justify-center mx-4 px-4 py-3 mt-5">
            <ul
                class="flex flex-wrap justify-between items-center mt-3 text-sm font-medium text-gray-500 dark:text-gray-400 sm:mt-0 text-center">
                <li class="px-4"></li>
                <li class="px-4">©{{ new Date().getFullYear() }} - ZapSys</li>
                <li class="px-4"><a :href="project_link" class="btn" target="_blank">
                        <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" width="32" height="32"
                            viewBox="0 0 30 30">
                            <path
                                d="M15,3C8.373,3,3,8.373,3,15c0,5.623,3.872,10.328,9.092,11.63C12.036,26.468,12,26.28,12,26.047v-2.051 c-0.487,0-1.303,0-1.508,0c-0.821,0-1.551-0.353-1.905-1.009c-0.393-0.729-0.461-1.844-1.435-2.526 c-0.289-0.227-0.069-0.486,0.264-0.451c0.615,0.174,1.125,0.596,1.605,1.222c0.478,0.627,0.703,0.769,1.596,0.769 c0.433,0,1.081-0.025,1.691-0.121c0.328-0.833,0.895-1.6,1.588-1.962c-3.996-0.411-5.903-2.399-5.903-5.098 c0-1.162,0.495-2.286,1.336-3.233C9.053,10.647,8.706,8.73,9.435,8c1.798,0,2.885,1.166,3.146,1.481C13.477,9.174,14.461,9,15.495,9 c1.036,0,2.024,0.174,2.922,0.483C18.675,9.17,19.763,8,21.565,8c0.732,0.731,0.381,2.656,0.102,3.594 c0.836,0.945,1.328,2.066,1.328,3.226c0,2.697-1.904,4.684-5.894,5.097C18.199,20.49,19,22.1,19,23.313v2.734 c0,0.104-0.023,0.179-0.035,0.268C23.641,24.676,27,20.236,27,15C27,8.373,21.627,3,15,3z">
                            </path>
                        </svg>
                    </a>
                </li>
            </ul>
        </footer>
    </div>
</template>
<script>
import functionPlot from 'function-plot'
export default {
    name: 'Raízes',
    data: () => ({
        coef_a: null,
        coef_b: null,
        coef_c: null,
        resposta: '',
        project_link: 'https://github.com/zapsys/equacoes-2-grau/',
        showModal: false
    }),
    watch: {
        showModal(val) {
            if (val) {
                this.plotGraph()
            }
        },
    },
    methods: {
        isNumber: function (evt) {
            evt = evt
            let charCode = (evt.which) ? evt.which : evt.keyCode
            if ((charCode > 31 && (charCode < 48 || charCode > 57)) && (charCode !== 45)) {
                evt.preventDefault()
            } else {
                return true
            }
        },
        equacao2Grau(a, b, c) {
            let delta, x1, x2, answer

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
        plotGraph() {
            const a = parseFloat(this.coef_a) || 0
            const b = parseFloat(this.coef_b) || 0
            const c = parseFloat(this.coef_c) || 0

            // Wait for the DOM to update before rendering the graph
            this.$nextTick(() => {
                functionPlot({
                    target: '#plot',
                    width: 500,
                    height: 400,
                    xAxis: { label: 'Eixo x' },
                    yAxis: { label: 'Eixo y' },
                    grid: true,
                    data: [
                        {
                            fn: `${a} * x^2 + ${b} * x + ${c}`,
                            graphType: 'interval',
                        },
                    ],
                });
            });
        },
    },
}
</script>