<script>
    export default {
        name: "Store"
    }

    export const store = {
        state: {
            canvasCircle: {
                x: 250,
                y: 250,
            }
        },
        changePosition(x, y) {
            this.state.canvasCircle.x = x;
            this.state.canvasCircle.y = y;
        }
    };
</script>

<style scoped>

</style>