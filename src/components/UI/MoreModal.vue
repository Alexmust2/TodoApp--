<template>
    <div class="modal__container">
        <slot></slot>
    </div>
</template>
<script>
export default {
    name: 'MoreModal'
}
</script>
<style lang="scss">
    .modal__container{
        width: 144px;
        height: 76px;
        display: flex;
        align-items: flex-start;
        flex-direction: column;
        justify-content: space-around;
        border: 1px solid #E3E5E8;
        background-color: #FFFFFF;
        position: absolute;
        text-align: center;
        margin-left: 180px;
        padding: 8px 0px;
        box-shadow: 0px 8px 8px rgba(0, 0, 0, 0.08), 0px 8px 16px rgba(0, 0, 0, 0.06);
        border-radius: 4px;
        
        button {
            width: 144px;
            height: 30px;
            display: flex;
            flex-direction: row;
            align-items: center;
            width: 100%;
            border: none;
            text-align: left;
            font-size: 14px;
            background: none;
            padding: 0;
            font-weight: 400;
            font-family: 'Inter', sans-serif;
            cursor: pointer;
        }

        button:hover {
            background: #E1F1FF;
        }

        img {
            width: 15px;
            margin-right: 10px;
            margin-left: 5px;
        }
    }
</style>