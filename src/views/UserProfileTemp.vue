<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{ user.username }}</h1>
            <div class="user-profile__admin-badge" v-if="user.isAdmin">
                Admin
            </div>
            <div class="user-profile__admin-badge" v-else>
                Not Admin
            </div>
            <div class="user-profile__follower-count">
                <strong>Followers: </strong> {{ followers }}
            </div> 
            <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot" :class="{ '--exceeded' : newTwootCharacterCount > 180}">
                <label for="newTwoot">
                    <strong>New Twoot</strong> ({{ newTwootCharacterCount}}/180)
                </label>
                <textarea id="newTwoot" rows="4" v-model="newTwootContent"></textarea>
                <div class="user-profile__create-twoot-type">
                    <label for="newTwootType">
                        <strong>Type: </strong>
                    </label> 
                    <select id="newTwootType" v-model="selectedTwootType">
                        <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                            {{ option.name }}
                        </option>
                    </select>
                </div>
                <button>
                    Twott!
                </button>
            </form> 
        </div>
        <div class="user-profile__twoots-wrapper">
            <TwootItem 
                v-for="twoot in user.twoots" 
                :key="twoot.id" :username="user.username" 
                :twoot="twoot" 
                @favourite="toggleFavourite"
            />
        </div>
    </div>
</template>

<script>
    import TwootItem from '../components/TwootItem.vue';
    export default {
        name: 'UserProfile',
        components: {TwootItem},
        data() {
            return {
                followers: 0,
                user: {
                    id: 1,
                    username: '_nickfryganiotis',
                    firstName: 'Nickolas',
                    lastName: 'Fryganiotis',
                    email: 'nickolasfryganiotis@example.com',
                    isAdmin: false, 
                    twoots: [
                        {id: 1, content: 'Twotter is amazing!'},
                        {id: 2, content: "Don't forget to subscribe to Niko's Channel"}
                    ]
                },
                twootTypes: [
                    {value: 'draft', name:'Draft'},
                    {value: 'instant', name:'Instant Twoot'}
                ],
                newTwootContent: '',
                selectedTwootType: 'instant'
            }
        },
        watch: {
            followers(newFollowerCount, oldFollowerCount) {
                if(oldFollowerCount < newFollowerCount) {
                    console.log(`${this.user.username} has gained a follower`)
                } 
            }
        }, 
        computed: {
            newTwootCharacterCount() {
                return this.newTwootContent.length 
            }
        },
        methods: {
            followUser() {
                this.followers++
            },
            toggleFavourite(id) {
                console.log(`Favourite twoot #${id}`)
            },
            createNewTwoot() {
                if(this.newTwootContent && this.selectedTwootType !== 'draft') {
                    this.user.twoots.unshift({
                        id: this.user.twoots.length + 1,
                        content: this.newTwootContent
                    })
                }
                this.newTwootContent = ''
            }
        },
        mounted() {
            this.followUser()
        }     
    }
</script>
<style lang="scss" scoped>
    .user-profile{
        display: grid;
        grid-template-columns: 1fr 3fr;
        width: 100%;
        padding: 50px 5%;
        
        .user-profile__user-panel {
            display: flex;
            flex-direction: column;
            margin-right: 50px;
            padding: 20px;
            background-color: white;
            border-radius: 5px;
            border: 1px solid #DFE3E8;
            
            h1 {
                margin: 0;
            }
            .user-profile__admin-badge {
                background: rebeccapurple;
                color: white;
                border-radius: 5px;
                margin-right: auto;
                padding:0 10px;
                font-weight: bold; 
            }

            .user-profile__create-twoot {
                border-top: 1px solid black;
                padding-top: 20px;
                display: flex;
                flex-direction: column;

                &.--exceeded {
                    color: red;
                    border-color: red;

                    button {
                        background-color: red;
                        border: none;
                        color: white;
                    }
                }
            }
        }
        
        .user-profile__twoots-wrapper {
            display: grid;
            grid-gap: 10px;
        }
        
    }
    
</style>