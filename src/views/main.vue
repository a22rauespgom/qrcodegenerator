<template>
    <div class="page">
        <header>
            <h1>QRCode Generator</h1>
            <hr>
        </header>
        <div class="qr-generator">
            <div class="form">
                <div class="input-container">
                    <label for="qr-value">QR Value</label>
                    <input id="qr-value" type="text" v-model="value" />
                </div>
                <div class="input-container">
                    <label for="qr-size">QR Size</label>
                    <input type="range" id="qr-size" min="200" max="600" v-model="size" list="tickmarks" step="100">
                    <datalist id="tickmarks">
                        <option v-for="index in 6" :value="index * 100"></option>
                    </datalist>
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
                    <label for="">QR Icon</label>
                    <label for="file-upload" class="custom-file-upload">
                        <span>Upload image</span>
                        {{ imageName || `No file uploaded` }}
                    </label>
                    <input id="file-upload" type="file" @change="handleFileChange" />
                </div>
            </div>
            <div class="qr">
                <div class="qr__container">
                    <qrcode-vue class="qr__code" :value="value" :size="size" level="H" render-as="svg"
                        :background="bgColor" :foreground="fgColor" margin="1" />
                    <img v-if="imageURL" class="qr__image" :src="imageURL" alt="" :style="imgBackground"
                </div>
                <button class="dl-button" @click="downloadQR">Download QR
                    <span class="material-symbols-rounded">download</span>
                </button>
            </div>
        </div>
        <footer>
            <span>
                Developed by <a href="https://github.com/a22rauespgom">Raúl Espinosa Gómez</a>
            </span>
        </footer>
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
            imageURL: '',
            imageName:'',
            showIcon:false,
        }
    },
    methods: {
        handleFileChange(e) {
            const file = e.target.files[0];
            if (file) {
                this.imageURL = URL.createObjectURL(file);
                this.imageName=file.name;
            }
        },
        downloadQR() {
            html2canvas(document.querySelector('.qr__container')).then(canvas => {
                const link = document.createElement('a');
                link.download = 'qr.png';
                link.href = canvas.toDataURL();
                link.click();
            });
        },
        deleteImage(){
            this.imageURL="";
            this.imageName="";
        }
    },
    computed:{
        imgBackground(){
            return{
                'background-color':this.bgColor,
                'border-color':this.bgColor
            }
        }
    }
}
</script>

<style scoped>
.page {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    width: 100vw;
    height: 100vh;
    padding: 20px;
}

header,
footer {
    width: 100%;
    text-align: center;
}

header h1{
    color:white;
    font-size: 2rem;
    font-variant: small-caps
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
    position: relative;
}

.qr__container {
    position: relative;
    transition: .3s ease-in-out;
    display: flex;
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
    margin-bottom: 0.2rem;
}

.input-container input[type="text"] {
    border-radius: 5px;
    font-size: 1.2rem;
    padding: .8rem;
}

input[type="file"] {
    display: none;
}

.custom-file-upload {
    border: 1px solid #000000;
    background-color: white;
    color: black;
    border-radius: 5px;
    display: inline-block;
    padding: .8rem;
}

.custom-file-upload span:not(.delete){
    padding:.6rem;
    border: gray;
    background-color: lightgray;
    cursor: pointer;
    border-radius: 5px;
    margin-right:1rem;
    transition: 0.2s ease-in-out;
}



.delete{
    /* position: static; */
    color: black;
}

.dl-button{
    margin:1rem 0;
    background-color: white;
    border: 1px solid black;
    border-radius: 5px;
    padding: .8rem;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: all .3s ease-in-out;
}


.dl-button span{
    margin-left:.5rem;
}

.dl-button:hover{
    background-color: gray;
    color: white;
}

</style>