<template>
    <div class="wrapper-popup" v-bind:class="getClassPopup">
        <div class="rule">
            <h3>Luật chơi</h3>
            <p>
                <ul>
                    <li>Điều chỉnh final score và ấn new game để chơi</li>
                    <li>Người chơi đổ xúc xắc và cộng dôn điểm tạm ,nếu muốn dừng lại để cộng vào điểm chính thì ấn HOLD</li>
                    <li>Nếu người chơi quay trúng mặt số 1 sẽ reset điểm tạm về 0 và chuyển tiếp lượt chơi cho người còn lại</li>
                    <li>         Cuối cùng ai đạt được điểm bằng với final score trước sẽ thắng</li>
                </ul>
                 
   
                
            </p>
            <button 
                v-on:click="confirm"
                class="confirm">Đã hiểu</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'popup-rule',
    props: {
        isOpenPopup: { type: Boolean, default: false }
    },
    data() {
        return {

        }
    },
    methods: {
        confirm() {
            console.log('confirm PopupRule.vue');
            this.$emit('handleConfirm')
        }
    },
    computed: {
        getClassPopup: function() {
            return {
                'open-popup': this.isOpenPopup
            }
        }
    }
}
</script>

<style>
    .wrapper-popup {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0,0,0,0.4);
        opacity: 0;
        visibility: hidden;
        transition: all .3s ease;
    }
    .wrapper-popup.open-popup {
        opacity: 1;
        visibility: visible;
    }
    .rule {
        width: 500px;
        padding: 20px;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%) scale(1.2);
        background-color: #fff;
        position: absolute;
        font-family: Arial, Helvetica, sans-serif;
        transition: all .3s ease;
    }
    .rule ul li{
        font-size: 19px;
    }
     .rule p{
         padding: 0 20px;
     }
    .open-popup .rule {
        transform: translate(-50%, -50%) scale(1);
    }
    /* scale -> class */
    .rule h3 {
        margin-bottom: 10px;
    }
    .rule .confirm {
        cursor: pointer;
        margin-top: 20px;
        padding: 8px 15px;
        border: 2px solid #333;
        background-color: #fff;
        transition: all .3s ease;
    }
    .rule .confirm:hover {
        color: #fff;
        background-color: #333;
    }
    /* kết hợp CSS3 với VueJs để xây dựng hiệu ứng - Zoom in - Zoom out */
</style>
