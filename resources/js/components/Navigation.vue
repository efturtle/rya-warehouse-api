<template>
    <div>
        <img src="/images/ryalogo.png" alt="RYA" class="w-8 md:w-12">
        <div class="flex flex-col mt-6">
            <aside id="nav">
            <ul>
                <li>
                    <router-link class="flex items-left py-2 text-gray-600 rounded-md hover:bg-gray-100 justify-center" to="/" >
                        <svg xmlns="http://www.w3.org/2000/svg" class="w-6 md:w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
                        </svg>
                    </router-link>
                </li>
                <li>
                    <router-link class="flex items-left py-2 mt-5 text-gray-600 rounded-md hover:bg-gray-100 justify-center" to="/tareas">
                        <svg xmlns="http://www.w3.org/2000/svg" class="w-6 md:w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01" />
                        </svg>
                    </router-link>
                </li>
                <li>
                    <router-link class="flex items-left py-2 mt-5 text-gray-600 rounded-md hover:bg-gray-100 justify-center" to="/herramientas">
                        <icons name="toolbox" />
                    </router-link>
                </li>
                <li v-if="isAdmin">
                    <router-link class="flex items-left py-2 mt-5 text-gray-600 rounded-md hover:bg-gray-100 justify-center" to="/peticiones">
                        <icons name="shopping-basket" />
                    </router-link>
                </li>
                <li v-if="isAdmin">
                    <router-link class="flex items-left py-2 mt-5 text-gray-600 rounded-md hover:bg-gray-100 justify-center" to="/usuarios">
                        <icons name="user-alt" />
                    </router-link>
                </li>
                <li>
                    <button class="flex items-left py-2 ml-2 mt-5 text-gray-600 rounded-md hover:bg-gray-100 justify-center" @click="logoutPrompt">
                        <icons name="user-astronaut" />
                    </button>
                </li>
            </ul>
            </aside>
        </div>
    </div>

</template>

<script>
import icons from 'v-svg-icons';
export default {
    components:{icons},
    data(){
        return {
            user: [],
        }
    },
    mounted(){
        // get the user
        axios.get('/api/user')
        .then((data) => {
            this.user = data.data;
        })
    },
    methods:{
        logoutPrompt(){
            this.$swal({
                title: 'Cerrar Session?',
                type: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Si',
                cancelButtonText: 'No',
                showCloseButton: true,
                showLoaderOnConfirm: true
            })
            .then((result) => {
                if (result.value) {
                    this.logout();
                }
            })
        },
        logout(){
            axios.post('/logout')
            .then(() => {
                window.location.href = "/login";
            })
            .catch(error => {
            })
        }
    },
    computed: {
        isAdmin(){
            if (this.user.puesto != 3) {
                return true;
            }
            return false;
        },
    }
}
</script>

<style>
#nav .active-link{
  color: #be6633;
}
</style>
