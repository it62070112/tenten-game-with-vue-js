<template>
    <div id="title_div">
        <!-- the app is here!, the message is "{{ message }}" <br /> -->
        <button @click="getRandomNumber()">new game</button>, score: {{ score_value }}, block grid:
        <div id="block_array_rendering" v-for="item in blockArray" v-bind:key="item.id">
            <!-- "{{ ((parseInt(item.id) + Math.pow(10, 2)) + '').substring(1, (2 + 1)) }}" - "{{ item.value }}", -->
            <div
                id="bloack_array_item_rendering"
                v-for="sub_item in split_row(item.value)"
                v-bind:key="sub_item.id"
                v-bind:style="[ sub_item.value != 1 ? {
                    backgroundColor: '#000',
                    color: '#FFF'
                } : {
                    backgroundColor: '#FFF',
                    color: '#000'
                } ]"
            >
                {{ sub_item.value != 1 ? item.id - 1 + sub_item.id : '__' }}
                <!-- {{ ['-', '##'][sub_item.value] }} -->
            </div>
        </div>
        <!-- random number: -->
        <!-- "{{ random_number }}" -->
        <!-- , offering -->
        your block choices editeds
        :
        <div id="block_offering">
            <div id="block_offering_array_rendering" v-for="item in selectionArray" v-bind:key="item.id">
                <!-- {{ block_offering_catalogue_array[item.value] }} <br /> -->
                <div
                    id="block_offering_item_rendering"
                    v-for="sub_item in decryptBlockOffering(block_offering_catalogue_array[item.value])"
                    v-bind:key="sub_item.id"
                >
                    <!-- "{{ splitOfferingRow(sub_item.value) }}" -->
                    <div
                        id="block_offering_row_rendering"
                        v-for="sub_sub_item in splitOfferingRow(sub_item.value)"
                        v-bind:key="sub_sub_item.id"
                        v-bind:style="[ sub_sub_item.value != '##' ? {
                            backgroundColor: '#000',
                            color: '#FFF'
                        } : {
                            backgroundColor: '#FFF'
                        } ]"
                    >
                        <!-- __ -->
                        {{ ((sub_item.id + sub_sub_item.id) + '') == '22' ? '[]' : '__' }}
                    </div>
                </div>
            </div>
        </div>
        input: <input v-model="form_model" v-on:keyup.enter="onEnter()" /> <br />
        Your Input: "{{ form_message }}", <br />
        Response Message: "{{ response_message }}",
        random_number: {{ random_number }}
    </div>
</template>

<script>
    export default {
        name: 'MainPage',
        props: {},
        data() {
            return {
                score_value: 0,
                random_number: '???',
                selectionArray: [
                    { "id": "1", "value": "??1" },
                    { "id": "2", "value": "??2" },
                    { "id": "3", "value": "??3" }
                ],
                blockArray: [
                    { "id": "1", "value": "17" },
                    { "id": "2", "value": "48" },
                    { "id": "3", "value": "95" },
                    { "id": "4", "value": "384" },
                    { "id": "5", "value": "650" },
                    { "id": "6", "value": "691" },
                    { "id": "7", "value": "778" },
                    { "id": "8", "value": "796" },
                    { "id": "9", "value": "933" },
                    { "id": "10", "value": "1003" }
                ],
                block_offering_catalogue_array: [
                    '--64', '3264', '3822', '1088',
                    '-192', '1092', '-224', '5188',
                    '-480', '5189', '-496', '1216',
                    '5568', '1120', '5232', '-100',
                    '-117', '-196', '-453'
                ],
                form_model: '',
                form_message: '...',
                response_message: '...'
            }
        },
        methods: {
            split_row: function (row_number) {
                let result = []
                for (let i = 0; i < 10; i++) {
                    result.push(
                        {
                            "id": (i + ""), "value": (
                                ((parseInt(row_number)
                                - (
                                    parseInt(row_number)
                                    % Math.pow(2, ((10 - 1) - i))
                                )) / Math.pow(2, ((10 - 1) - i)))
                                % 2
                            )
                        }
                    )
                }
                return result
            },
            getRandomNumber: function () {
                let result_string = ((
                    Math.floor(Math.random() * Math.pow(10, 4))
                    + Math.pow(10, 4)
                ) + '').substring(1, (4 + 1));
                this.random_number = result_string;
                this.selectionArray = this.generateSelectionArray(result_string);
                let result_array = [];
                for (let i = 0; i < 10; i++) {
                    result_array.push(
                        { "id": ((i + 1) + ''), "value": "0" }
                    )
                }
                this.blockArray = result_array;
            },
            generateSelectionArray: function(number_input) {
                let result_array = [];
                for (let i = 0; i < 3; i++) {
                    result_array.push(
                        { "id": ((i + 1) + ''), "value": ((
                            parseInt(number_input) % Math.pow(19, 3)
                            - (parseInt(number_input) % Math.pow(19, 3) % Math.pow(19, ((3 - 1) - i)))
                        ) / Math.pow(19, ((3 - 1) - i))) % 19 }
                    )
                }
                return result_array;
            },
            decryptBlockOffering: function(undecrypted_block_offering_input) {
                let undecrypted_block_offering = '---0';
                if (undecrypted_block_offering_input) {
                    undecrypted_block_offering = undecrypted_block_offering_input;
                }
                undecrypted_block_offering = undecrypted_block_offering.replace(/-/g, '0');
                let undecrypted_block_offering_as_number = parseInt(undecrypted_block_offering);
                let result_array = [];
                let indexing_counter = (((Math.pow(5, 2) + 1) / 2) - 1) + Math.pow(10, 2);
                for (let i = 0; i < 5; i++) {
                    let value_array = [];
                    for (let j = 0; j < 5; j++) {
                        let is_usable = (
                            (
                                (
                                    ((5 - 1) / 2)
                                    - Math.abs(((5 - 1) / 2) - j)
                                )
                                - (
                                    Math.abs(((5 - 1) / 2) - i)
                                )
                            ) + 1
                        );
                        let indexing_string = '';
                        if (is_usable > 0) {
                            if ((((
                                undecrypted_block_offering_as_number
                                - (undecrypted_block_offering_as_number % Math.pow(2, (
                                    indexing_counter - Math.pow(10, 2)
                                )))
                            ) / Math.pow(2, (indexing_counter - Math.pow(10, 2)))) % 2) == 1) {
                                indexing_string = '##'
                            }
                            if ((((
                                undecrypted_block_offering_as_number
                                - (undecrypted_block_offering_as_number % Math.pow(2, (
                                    indexing_counter - Math.pow(10, 2)
                                )))
                            ) / Math.pow(2, (indexing_counter - Math.pow(10, 2)))) % 2) == 0) {
                                indexing_string = '--'
                            }
                            indexing_counter -= 1;
                        }
                        if (is_usable <= 0) {
                            indexing_string = '__';
                        }
                        value_array.push(indexing_string);
                    }
                    result_array.push(
                        { "id": (i + ''), "value": value_array }
                    )
                }
                return result_array;
            },
            splitOfferingRow: function(array_input) {
                let result_array = [];
                for (let i = 0; i < array_input.length; i++) {
                    result_array.push(
                        { "id": (i + ''), "value": array_input[i] }
                    )
                }
                return result_array;
            },
            onEnter: function() {
                this.form_message = this.form_model;
                this.form_model = '';
                let valid_input = 1;
                let reason_string = '';
                let reason_array = [];
                for (let i = 0; i < this.form_message.length; i++) {
                    if (
                        (this.form_message.charCodeAt(i) < 48)
                        || (this.form_message.charCodeAt(i) > 57)
                    ) {
                        valid_input = 0;
                        reason_array.push("your input contains non-numerical character");
                        i = (this.form_message.length - 1);
                    }
                }
                if (this.form_message.length != 3) {
                    valid_input = 0;
                    reason_array.push("your input should be exactly 3 characters long")
                }
                if (valid_input == 1) {
                    if ((parseInt(this.form_message) < 100) || (parseInt(this.form_message) > 399)) {
                        valid_input = 0;
                        reason_array.push(
                            "the first character of your input should be a number between 1 and 3,"
                            + " "
                            + "and your second to third charcters of your input should be a number "
                            + "between '00' and '99'."
                        )
                    }
                }
                if (this.random_number == '???') {
                    valid_input = 0;
                    reason_array = ["You haven't yet started the game."]
                }
                for (let i = 0; i < reason_array.length; i++) {
                    reason_string += reason_array[i]
                    if ((i + 1) < reason_array.length) {
                        reason_string += ', '
                    }
                }
                if ((reason_array.length == 0) || (valid_input == 1)) {
                    reason_string = 'no error';
                }
                this.response_message = (
                    'your input is: \''
                    + reason_string
                    + '\''
                );
            }
        }
    }
</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Inconsolata&display=swap');
    #title_div {
        font-weight: 900;
        width: 80%;
        position: relative;
        left: 10%;
        background-color: #BBB;
        padding: 0.1vw;
    }
    #block_array_rendering {
        background-color: #888;
        position: reletive;
        color: black;
        margin: 0.1vw;
        padding-left: 31vw;
    }
    #bloack_array_item_rendering {
        display: inline-block;
        background-color: #BBB;
        margin: 0.1vw;
        width: 1.5vw;
        height: 1.5vw;
        text-align: center;
        font-size: 0.8vw;
    }
    #block_offering {
        background-color: #888;
        margin: 0.1vw;
        text-align: center;
        /* padding: 0.1vw; */
    }
    #block_offering_array_rendering {
        background-color: #BBB;
        margin: 0.1vw;
        width: 10vw;
        /* height: 10vw; */
        display: inline-block;
        font-family: 'Inconsolata', monospace;
    }
    #block_offering_item_rendering {
        background-color: #888;
        margin: 0.1vw;
        padding: 0.1vw;
    }
    #block_offering_row_rendering {
        background-color: #BBB;
        margin: 0.1vw;
        display: inline-block;
    }
</style>
