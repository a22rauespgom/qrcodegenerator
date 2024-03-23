<template>
    <div>
        <div class="color-container">
            <button :class="['color', 'color-button', { 'selected': colorSelected === updatedColor.substring(1) }]"
                v-for="colorSelected in colors" :key="colorSelected" :style="{ backgroundColor: '#' + colorSelected }"
                @click="selectColor(colorSelected)">
            </button>
            <!-- <div class="color selected-color" :style="{ backgroundColor: updatedColor }"></div> -->
            <div class="input-container">
                <label for="colorInput" class="static-hash"
                    :style="[colorCode, { backgroundColor: updatedColor }]">#</label>
                <input id="colorInput" type="text" :placeholder="updatedColor.substring(1)"
                    @input="selectColor($event.target.value)">
            </div>
        </div>
    </div>
</template>

<script>
import tinycolor from 'tinycolor2';

export default {
    data() {
        return {
            colors: ['8C001A', 'FF3B30', 'ED5603', 'DC136C', 'FFBF00', 'F4D06F', 'F2E94E', '392F5A', '610F7F', 'F2A6A6', 'B7AD99', '0CCE6B', '06A77D', '96E6B3', '82DDF0', '1098F7'],
            updatedColor: ''
        };
    },
    props: {
        color: {
            type: String,
            default: 'ED5603'
        }
    },
    created() {
        if (this.color.startsWith('#') && this.color.length > 1) {
            // Si empieza con '#' y tiene al menos dos caracteres, quitamos el primer caracter y devolvemos el resto del string
            this.updatedColor = this.color.slice(1);
        } else {
            // Si no cumple las condiciones anteriores, devolvemos el mismo string
            this.updatedColor = this.color;
        }
        this.updatedColor = `#${this.updatedColor}`
    },
    methods: {
        selectColor(color) {
            this.updatedColor = '#' + color;
            this.$emit('update:color', this.updatedColor);
        },
    },
    computed: {
        colorCode() {
            const color = tinycolor(this.updatedColor);
            if (color.isDark()) {
                return {
                    color: '#fff'
                }
            } else {
                return {
                    color: '#000'
                }
            }
        }
    }
}
</script>

<style scoped>
.color-container {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    margin: 5px;
    background-color: white;
    box-shadow: 0 1px 4px rgba(0, 0, 0, .25);
}

.color {
    border-radius: 4px;
    margin: 5px;
    width: 30px;
    height: 30px;
}

.color-button {
    transition: all 0.3s ease-in-out;
    border: 1px solid transparent;
    cursor: pointer;
}

.input-container {
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    margin: 5px;
    /* Asegura que el input y el label estÃ©n en la misma lÃ­nea */
}

.input-container .static-hash {
    align-items: center;
    background: #f0f0f0;
    border-radius: 4px 0 0 4px;
    color: #98a1a4;
    display: flex;
    float: left;
    height: 30px;
    justify-content: center;
    width: 30px;
}

#colorInput {
    border: 0;
    border-radius: 0 4px 4px 0;
    box-shadow: inset 0 0 0 1px #f0f0f0;
    box-sizing: content-box;
    color: #666;
    float: left;
    font-size: 14px;
    height: 28px;
    outline: none;
    padding: 1px 1px 1px 8px;
    width: 100px;
}

.selected {
    border: 3px solid #98a1a4;
    width:45px;
    height:45px;
}
</style>
