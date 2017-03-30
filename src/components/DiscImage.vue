<style scoped>

img {
    width: 100px;
}

</style>

<template>

<div>
    <img :src='imageURL'></img>
</div>

</template>

<script>

export default {

    data() {
        return {
            "imageURL": "https://infinitediscs.com/Inf_Uploads/Disc_Images/DI_granite.JPG"

        }
    }
}

</script>
