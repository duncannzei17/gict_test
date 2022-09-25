<template>
    <v-card outlined>
        <v-data-table
        :headers="headers"
        :items="users"
        :items-per-page="5"
        class="elevation-1"
        >   
            <template v-slot:item.controls="props">
                <v-btn class="mx-2" small @click="onButtonClick(props.item)">
                <v-icon dark>mdi-pencil</v-icon>
                </v-btn>
            </template>
      
        </v-data-table>
    </v-card>
</template>

<script>
import axios from 'axios'

export default {
 
    data: () => {
        return {
        headers: [
            {
            text: 'ID',
            align: 'start',
            value: 'ID',
            },
            { text: 'Message', value: 'Message',sortable: false, },
            { text: 'Age', value: 'Age' },
            { text: '', value: 'controls', sortable: false }
        ],
        users:[],
        }
    },
    mounted() {
        // setTimeout('', 5000);
        this.loadData()
                        
    },
    methods: {
        loadData () {
                let temp = []
                axios.get('http://localhost:8080/api/dummy/items/', {
                headers: {'Authorization': 'Bearer '.concat('ALDJAK23423JKSLAJAF23423J23SAD3')},
                }).then((response) => {
                    this.users = response.data
                }).catch(function (error) {
                if (error.response) {console.log(error.response.headers);} 
                else if (error.request) {console.log(error.request);} 
                else {console.log(error.message);}
                }) 
                return temp;
        },
        onButtonClick(item) {
            //edit button
            console.log('click on ' + item.ID)
        }
    }
}
</script>
