<template>
    <div class="motivation">
        <!-- header -->
        <div class="motovation__header">
            <div class="header-content">
                <h1>Система мотивации</h1>
                <p>Система мотивации - это механизм, стимулирующий участников совершенствовать 
                    свои навыки и развивать Проектное сообщество. Система состоит из набора заданий, 
                    за выполнение которых участники могут получать призы
                </p>
            </div>
        </div>

        <!-- accordeon 1 -->
        <components-motivation-accardeon id="accordeon-open1" title="Зачем нужна система мотивации?" :user="user">
            <template v-slot:content>
                <div class="ui-kit__accardeon-content">
                    <components-motivation-accordeion-content-1 />
                </div>
            </template>
        </components-motivation-accardeon>

        <!-- accordeon 2 -->
        <components-motivation-accardeon id="accordeon-open2" title="Как это работает?" :user="user">
            <template v-slot:content>
                <div class="ui-kit__accardeon-content">
                    <components-motivation-accordeion-content-2 />
                </div>
            </template>
        </components-motivation-accardeon>

        <!-- accordeon 2 -->
        <components-motivation-accardeon id="accordeon-open3" title="Какие награды получают участники?" :user="user" >
            <template v-slot:content >
                <div class="ui-kit__accardeon-content">
                    <components-motivation-accordeion-content-3 />
                </div>
            </template>
        </components-motivation-accardeon>

        <template v-if="user">

            <!-- motivation progress cards-->
            <components-motivation-progress>
                <!-- card 1 -->
                <template v-slot:card1 >
                    <components-motivation-card-1  :params="card1Params" />
                </template>

                <!-- card 2 -->
                <template v-slot:card2 >
                    <components-motivation-card-block v-if="secondCardBlock" />
                    <components-motivation-card-2 v-else  :params="card2Params" />
                </template>
                
                <!-- card 3 -->
                <template v-slot:card3 >
                    <components-motivation-card-double @openModal="modalOpen" :bages="bages" />
                </template>
            </components-motivation-progress>

            <!-- Задания -->
            <div class="motivation__task-switch">
                <div class="tasks_title" >
                    <h3>Задания</h3>
                </div>

                <div class="tasks_cards">
                    <div class="card-active-switch" @click="switcher = 0, achievement = 0, secondCardBlock = true"
                        :class="{'switch-active': switcher === 0}">
                        <span>Карта активности</span>
                    </div>
                    <div class="card-active-involve" @click="switcher = 1, achievement = 4, secondCardBlock = false"
                        :class="{'switch-active': switcher === 1}">
                        <span>Карта вовлеченности</span>
                    </div>
                </div>
            </div>

            <!-- Правила -->
            <div class="motivation__rules">
                <h4>Подтверждение задания</h4>
                <p>После того, как вы выполните задание, необходимое для получения бейджа, 
                    отправьте администраторам подтверждение. Это может быть скриншот с результатом 
                    вашей работы или ссылка на ваше выступление, проект или идею (зависит от задания). 
                    Если задание можно выполнить не единожды, необходимо отправлять подтверждение каждый 
                    раз, как вы его сделаете.</p>
            </div>
            
            <!-- Блок выбора заданий -->
            <template v-if="switcher === 0">
                <components-motivation-active-tasks @selectActiveTasks="activateTasks" />
            </template>
            <template v-else-if="switcher === 1">
                <components-motivation-envolve-tasks @selectActiveTasks="activateTasks" />
            </template>

            <!-- Блок заданий -->
            <components-motivation-tasks-block :challenges="achievements[achievement]" />

            <!-- модальное окно -->
            <components-motivation-modal :openModal="openModal" @closeModal="modalClose"/>

        </template>
    </div>
</template>
<script>

export default ({
    name: 'Motivation',
    props: ['user', 'bages'],
    data () {
        return {
            // Заблокирована ли 2 карта
            secondCardBlock: true,
            //переключатель заданий карточки
            switcher: 0,
            // data challenge items
            openModal: false,
            // active achievement block
            achievement: 0,

            // собранные бейджи
            // bages: 6,

            // параметры заполнения карточек
            card1Params: {
                ourPeople: 70, digitalDoctor: 90, ambassador: 15, expert: 0
            },
            card2Params: {
                teammate: 70, ideaGenerator: 90, heartthrob: 15, superStar: 0
            },

            // all achievements
            achievements: [],
            
        }
    },
    methods: {

        // открытие модального окна с наградами
        modalClose(data) {
            this.openModal = data.modal
        },
        // закрытие модального окна с наградами
        modalOpen(data) {
            this.openModal = data.modal
        },

        // переключение блока с заданиями Карты активности
        activateTasks(data) {
            this.achievement = data.task;
        },
    },
    
    beforeMount() {
        this.achievements = JSON.parse (this.bages)
    }
})
</script>