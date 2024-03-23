<template>
    <div class="page">
        <header>
            <h1>QR CODE GENERATOR</h1>
        </header>
        <div class="qr-generator">
            <div class="form">
                <div class="input-container">
                    <label for="qr-value">QR Value</label>
                    <input id="qr-value" type="text" v-model="value" />
                </div>
                <div class="input-container">
                    <label for="qr-size">QR Size</label>
                    <input type="range" id="qr-size" min="200" max="600" v-model="size">
                </div>
                <div class="input-container">
                    <label for="">Background Color</label>
                    <color :color="bgColor" @update:color="bgColor = $event" />
                </div>
                <div class="input-container">
                    <label for="">Foreground Color</label>
                    <color :color="fgColor" @update:color="fgColor = $event" />
                </div>
                <div class="input-container">
                    <label for="image-value">QR Icon</label>
                    <input id="image-value" type="file" @change="handleFileChange">
                </div>
            </div>
            <div class="qr">
                <div class="qr__container">
                    <qrcode-vue class="qr__code" :value="value" :size="size" level="H" render-as="svg"
                        :background="bgColor" :foreground="fgColor" margin="1" />
                    <img v-if="imageURL" class="qr__image" :src="imageURL" alt="">
                </div>
            </div>
        </div>
        <footer>Developed by <a href="https://github.com/a22rauespgom">Raúl Espinosa Gómez</a></footer>
    </div>
</template>

<script>
import QrcodeVue from 'qrcode.vue';
import color from '@/components/color.vue';
import html2canvas from 'html2canvas';

export default {
    components: {
        QrcodeVue,
        color
    },
    data() {
        return {
            value: 'Hello World!',
            size: 500,
            bgColor: '#ffffff',
            fgColor: '#000000',
            imageURL: ''
        }
    },
    methods: {
        handleFileChange(e) {
            const file = e.target.files[0];
            if (file) {
                this.imageURL = URL.createObjectURL(file);
            }
        },
        donwloadQR() {
            html2canvas(document.querySelector('.a')).then(canvas => {
                const link = document.createElement('a');
                link.download = 'qr.png';
                link.href = canvas.toDataURL();
                link.click();
            });
        }
    }
}
</script>

<style scoped>
.page {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100vw;
    height: 100vh;
    margin-top: 20px; /* Ajusta el margen superior para el header */
    /* Ajusta el margen inferior para el footer */
}


.qr-generator {
    display: grid;
    grid-template-columns: 1fr .8fr;
}

.qr {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
}

.qr__container {
    position: relative;
}

.qr__container:hover {
    /* opacity: 50%; */
}

.qr__image {
    background-color: #fff;
    border: .5rem solid #fff;
    border-radius: 0.25rem;
    /* box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.25); */
    height: var(--main-size);
    left: 50%;
    overflow: hidden;
    position: absolute;
    top: 50%;
    transform: translate(-50%, -50%);
    width: var(--main-size);
}

.form {
    display: flex;
    justify-content: center;
    /* text-align: center; */
    flex-direction: column;
    margin: 2rem;
}

.input-container {
    display: flex;
    flex-direction: column;
    margin: 1rem;
}

.input-container label {
    font-size: 1.1rem;
}

.input-container input[type="text"] {
    border-radius: 5px;
    width: 50%;
}
</style>