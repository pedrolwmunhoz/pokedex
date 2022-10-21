<template>
    <div class="header">
        <div class="logo">
            <img src="https://pokeweak.github.io/images/logo.svg"/>
            <h1>Copybase</h1>
        </div>
        <p>Developed by Pedro Munhoz</p>
    </div>
</template>


<style lang="scss" scoped>
    .header {
        width: 100%;
        height: 8vh;
        border-bottom: solid 1px rgb(190, 199, 206);
        display: flex;
        justify-content: space-between;
        align-items: center;
        background-color: rgb(253,255,255);
        box-shadow: 1px 1px 8px rgb(140, 147, 155);
        color: rgb(53,70,84);
        gap: 10%;
        img {
            width: 40px;
            height: 40px;
        }
        p {
            text-align: center;
            max-width: 10%;
            @media screen and (max-width: 600px){
                font-size: 0.8rem;
                text-align: center;
                max-width: 30%;
            }
        }
    }
    .logo {
        display: flex;
        gap: 15px;
        flex-direction: row;
        align-items: center;

        margin-left: 20px ;
        img {
            width: 40px;
            height: 40px;
        }
    
    }
</style>