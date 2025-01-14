<template>
    <div class="wrapper">
        <h1>Weather App</h1>
        <p>Check weather in your city</p>
        <form>
            <input type="text" placeholder="Type your city..." aria-label="Type your city" />
            <button type="button">Check</button>
        </form>
    </div>
</template>

<style lang="scss" scoped>
@use "@/assets/scss/styles" as *;

.wrapper {
    background-color: #FFFFFF;
    border-radius: 24px;
    box-shadow: 0 40px 32px -32px #0F0F0F29;
    color: #1D1D1D;
    height: 500px;
    padding: 20px;
    text-align: center;
    width: 640px;

    h1 {
        font-size: 36px;
        margin-top: 50px;
    }

    p {
        font-size: 16px;
        margin-top: 20px;
    }

    form {
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
        margin: 0 100px;
    }

    input {
        background-color: #FFFFFF;
        border: 1px solid #CFCFCF;
        border-radius: 8px;
        caret-color: #1D1D1D;
        color: #1D1D1D;
        font-size: 16px;
        line-height: 1.25;
        padding: 15px;
        margin-top: 30px;
        transition: border-color .3s;
        width: 100%;

        &:hover,
        &:focus {
            border-color: #1C1D1F;
        }
    }

    button {
        appearance: none;
        cursor: pointer;
        border: none;
        background-color: #5280FF;
        color: #FFFFFF;
        border-radius: 8px;
        font-size: 16px;
        line-height: 1.15;
        margin-top: 40px;
        padding: 15px 25px;
        width: 100%;
        transition: background .3s, transform .3s, filter .3s;

        &:hover {
            filter: brightness(92%);
        }

        &:active {
            transform: scale(0.96);
        }
    }
}

</style>

<script>

</script>
