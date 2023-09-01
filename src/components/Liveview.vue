<template>
    <div class="Liveview">
        <canvas ref="canvas" :width="canvasSize.width" :height="canvasSize.height"></canvas>
    </div>
</template>

<script>
export default {
    name: 'Liveview',
    props: {
        liveviewData: {
            type: Object,
            required: true,
        },
    },
    mounted() {
        this.canvas = this.$refs.canvas;
        this.ctx = this.canvas.getContext('2d');
        this.drawCanvas(); // Call the drawing method
    },
    watch: {
        liveviewData: 'drawCanvas', // Watch the liveviewData prop and call drawCanvas when it changes
    },
    computed: {
        canvasSize() {
            if (this.liveviewData.length == 0) {
                return { width: 20, height: 20 };
            } else {
                return {
                    width: this.canvasSettings.rectWidth * this.canvasSettings.columns + this.canvasSettings.padding * (this.canvasSettings.columns - 1),
                    height: this.canvasSettings.rectHeight * this.canvasSettings.rows + this.canvasSettings.padding * (this.canvasSettings.rows - 1),
                };
            }
        },
    },
    data() {
        return {
            canvas: null,
            ctx: null,
            canvasSettings: {
                rows: this.liveviewData.height,
                columns: this.liveviewData.width,
                rectWidth: 10,
                rectHeight: 10,
                padding: 1,
            },
        };
    },
    methods: {
        drawCanvas() {
            console.log('Drawing canvas from state');
            const rectWidth = this.canvasSettings.rectWidth;
            const rectHeight = this.canvasSettings.rectHeight;
            const padding = this.canvasSettings.padding;
            const rows = this.canvasSettings.rows;
            const columns = this.canvasSettings.columns;

            this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);

            for (let row = 0; row < rows; row++) {
                for (let col = 0; col < columns; col++) {
                    const index = row * columns + col;
                    const x = col * (rectWidth + padding);
                    const y = row * (rectHeight + padding);
                    const color = this.liveviewData.data[index];
                    this.ctx.fillStyle = decimalToHex(color);
                    this.ctx.fillRect(x, y, rectWidth, rectHeight);
                }
            }
        },
    },
};

function decimalToHex(rgbDecimal) {
    // Separate the RGB decimal values
    const red = Math.floor(rgbDecimal / (256 * 256));
    const green = Math.floor((rgbDecimal % (256 * 256)) / 256);
    const blue = rgbDecimal % 256;

    // Convert decimal to hexadecimal with leading zeros
    const redHex = red.toString(16).padStart(2, '0');
    const greenHex = green.toString(16).padStart(2, '0');
    const blueHex = blue.toString(16).padStart(2, '0');

    // Concatenate and return the hexadecimal color
    const hexColor = `#${redHex}${greenHex}${blueHex}`;
    return hexColor;
}
</script>

<!-- <style>
 [log-text] {
     line-height: 1.4rem !important;
     font-size: 14px;
     color: black;
     resize: none;
 }
 </style> -->
