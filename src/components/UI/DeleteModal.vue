<template>
    <div class="DeleteModal">
        <div class="deletemodal__container">
            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    name: 'DeleteModal'
}
</script>

<style lang="scss">
    .DeleteModal {
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.6);
        position: absolute;
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 55;
        margin: 0;
        left: 0px;
        top: 0px;
    }
    .deletemodal__container{
        display: flex;
        flex-direction: row;
        align-items: flex-start;
        padding: 24px 40px 40px;
        isolation: isolate;

        position: absolute;
        width: 500px;
        height: 196px;

        color: black;


        background: #FFFFFF;
        box-shadow: 0px 16px 32px rgba(0, 0, 0, 0.1), 0px 32px 48px rgba(0, 0, 0, 0.16);
        border-radius: 8px;
    }
</style>