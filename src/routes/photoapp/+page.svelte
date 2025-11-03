<script>  
let image = "https://cdn.pixabay.com/photo/2020/02/08/14/36/trees-4830285_640.jpg"
let photo = "https://c.wallhere.com/photos/e8/f1/1230x768_px_Calm_Cold_forest_Hill_lake_landscape_nature-620411.jpg!d"
let picture = "https://t4.ftcdn.net/jpg/05/32/61/65/360_F_532616585_ioZ554c9ChgrdtWMeACJFzA5QyuaJSZd.jpg"
let b1 = "https://tse2.mm.bing.net/th/id/OIP.QMNf8jzzh9jYVbTyVNyA-AHaEh?rs=1&pid=ImgDetMain&o=7&rm=3"
let b2 = "https://img.freepik.com/premium-photo/snowcovered-forest-illustration_863013-140051.jpg"
let b3 = "https://img.freepik.com/premium-photo/filtering-warm-afternoon-light-into-mosaic-soft-illumination_574745-1618.jpg"

</script>

<main>
    <h1>Photoapp</h1>
    <p>Denna sida innehåller fina vinter bilder.</p>
</main>

<div class="photos">
    <div class="image-wrapper">
        <img src={image} alt="Bild 1">
    </div>

    <div class="image-wrapper">
        <img src={photo} alt="Bild 2">
    </div>

    <div class="image-wrapper">
        <img src={picture} alt="Bild 3">
    </div>

    <div class="image-wrapper">
        <img src={b1} alt="Bild 4">
    </div>

    <div class="image-wrapper">
        <img src={b2} alt="Bild 5">
    </div>

    <div class="image-wrapper">
        <img src={b3} alt="Bild 6">
    </div>
</div>

<style>

    h1{
        text-align: center;
        font-size: 50px;
        margin: 20px;
        color: pink;
    }

    p{text-align: center;
        font-size: 30px;
        margin: 20px;
        color: rgb(253, 194, 201);
    }

    .photos{
        display:flex;
        flex-wrap: wrap;
        max-width: 90vw;
        align-items:center;
        justify-content:center;
        background-color: rgb(255, 246, 246);
    }
.image-wrapper {
    flex: 0 0 30%;
    margin: 10px;
    height: 40vh;

    
}

.image-wrapper img {
    width: 100%;
    height: 100%;
    display: block;
    border-radius: 20px;
    border: 2px solid pink;
    box-shadow: 4px 8px 8px rgba(0,0,0,0.2);
}

.image-wrapper:hover { flex-grow:2; } 
.image-wrapper:active { transform: scale(0.9); }

</style>
