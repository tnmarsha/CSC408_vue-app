<template>

    <Page>

        <ActionBar :title="'Customers that rented ' + movie_title" android:flat="true"/>

        <ListView for="rental in rentals" class="list-group" style="height:100%">

            <v-template>

                <movie-rental-history :rental="rental"></movie-rental-history>

            </v-template>

        </ListView>

    </Page>

</template>

<script>

    function Rental({id, name, email, pivot}) {

        this.id = parseInt(pivot.id)

        this.name = name

        this.email = email

        this.transaction = pivot

    }

    import axios from "axios";

    import MovieRentalHistory from '@/components/MovieRentalHistoryComponent.vue'



    export default {

        data() {

            return {

                movie_title: '',

                rentals: []

            }

        },

        methods: {

            read() {

                let url = 'https://codeflare.tech/api/movies/' + this.id + '/rentals'

                axios.get(url).then(({data}) => {

                    this.movie_title = data[0].title;

                    console.log('Name is: ' + this.movie_title + '-------');

                    console.log(data[0]);

                    data[0].rentals.forEach(rental => {

                        this.rentals.push(new Rental(rental))

                    });

                }, error => {

                    console.error(error);

                })

            }

        },

        components: {

            MovieRentalHistory

        },

        props: ['id'],

        created() {

            this.read()

        }

    }

</script>



<style scoped lang="scss">

    ActionBar

    {

        background-color: #5CDB95;

        color: #EDF5E1;

    }

</style>