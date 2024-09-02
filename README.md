<!DOCTYPE html>

<html lang="ar">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Mohest</title>

    <style>

        body {

            font-family: Arial, sans-serif;

            direction: rtl;

            text-align: center;

            background-color: #121212;

            color: #ffffff;

            margin: 0;

            padding: 0;

        }

        .search-bar {

            margin: 20px;

        }

        .search-bar input {

            width: 100%;

            padding: 10px;

            border-radius: 5px;

            border: 1px solid #444;

            background-color: #333;

            color: #ffffff;

        }

        .gallery {

            display: grid;

            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));

            gap: 10px;

            padding: 20px;

            max-width: 1200px;

            margin: auto;

        }

        .gallery-item {

            position: relative;

            border: 2px solid #444;

            border-radius: 10px;

            overflow: hidden;

        }

        .gallery img {

            width: 100%;

            height: auto;

            object-fit: cover;

            cursor: pointer;

            transition: transform 0.3s ease;

        }

        .gallery img:hover {

            transform: scale(1.05);

        }

        .gallery .caption {

            background-color: rgba(0, 0, 0, 0.7);

            color: #ffffff;

            padding: 5px;

            position: absolute;

            bottom: 0;

            width: 100%;

            text-align: center;

            box-sizing: border-box;

            font-size: 14px;

        }

        .upload-section {

            position: fixed;

            top: 20px;

            right: 20px;

            z-index: 100;

        }

        .upload-section input[type="text"] {

            margin-bottom: 10px;

            padding: 10px;

            border-radius: 5px;

            border: 1px solid #444;

            background-color: #333;

            color: #ffffff;

            width: 80%;

        }

        .upload-section button {

            padding: 10px 20px;

            cursor: pointer;

            border: none;

            border-radius: 5px;

            background-color: #4CAF50;

            color: white;

            margin-top: 10px;

            transition: background-color 0.3s ease;

        }

        .upload-section button:hover {

            background-color: #45a049;

        }

        .fullscreen {

            display: none;

            position: fixed;

            top: 0;

            left: 0;

            width: 100%;

            height: 100%;

            background-color: rgba(0, 0, 0, 0.8);

            justify-content: center;

            align-items: center;

            z-index: 1000;

        }

        .fullscreen img {

            max-width: 90%;

            max-height: 90%;

        }

        .fullscreen .actions {

            margin-top: 20px;

        }

        .fullscreen .actions button {

            margin: 0 10px;

            padding: 10px 20px;

            cursor: pointer;

            border: none;

            border-radius: 5px;

            background-color: #4CAF50;

            color: white;

            transition: background-color 0.3s ease;

        }

        .fullscreen .actions button:hover {

            background-color: #45a049;

        }

        .toggle-theme {

            position: fixed;

            top: 20px;

            left: 20px;

            background-color: #4CAF50;

            color: white;

            padding: 10px 20px;

            border-radius: 5px;

            cursor: pointer;

            border: none;

            z-index: 100;

            transition: background-color 0.3s ease;

        }

        .toggle-theme:hover {

            background-color: #45a049;

        }

        .theme-icon {

            margin-right: 10px;

        }

    </style>

</head>

<body onload="loadImages()">

    <h1>Mohest</h1>

    <!-- زر تغيير الوضع -->

    <button class="toggle-theme" onclick="toggleTheme()">

        <span class="theme-icon">🌙</span> تغيير الوضع

    </button>

    <!-- زر إضافة صورة ثابت -->

    <div class="upload-section">

        <input type="file" id="uploadInput" accept="image/*" style="display:none;" onchange="showTitleInput()">

        <input type="text" id="imageTitleInput" placeholder="أدخل عنوان الصورة" style="display:none;">

        <button id="uploadButton" onclick="document.getElementById('uploadInput').click()">+ اختر صورة</button>

        <button id="submitButton" onclick="uploadImage()" style="display:none;">رفع الصورة</button>

    </div>

    <!-- قسم البحث -->

    <div class="search-bar">

        <input type="text" id="searchInput" placeholder="ابحث عن صورة..." onkeyup="searchImages()">

    </div>

    <!-- معرض الصور -->

    <div class="gallery" id="gallery">

        <!-- المعرض فارغ بانتظار رفع الصور -->

    </div>

    <!-- عرض الصورة في كامل الشاشة -->

    <div class="fullscreen" id="fullscreen">

        <img id="fullscreenImg" src="" alt="عرض الصورة">

        <div class="actions">

            <button onclick="saveImage()">حفظ الصورة</button>

            <button onclick="closeFullscreen()">❌️ إغلاق</button>

        </div>

    </div>

    <script>

        // تغيير بين الوضع الليلي والوضع الأبيض بضغطة واحدة

        function toggleTheme() {

            let body = document.body;

            let themeIcon = document.querySelector('.theme-icon');

            if (body.style.backgroundColor === 'rgb(18, 18, 18)' || body.style.backgroundColor === '') {

                body.style.backgroundColor = '#ffffff';

                body.style.color = '#000000';

                themeIcon.textContent = '☀';

            } else {

                body.style.backgroundColor = '#121212';

                body.style.color = '#ffffff';

                themeIcon.textContent = '🌙';

            }

        }

        // البحث عن الصور

        function searchImages() {

            let input = document.getElementById('searchInput').value.toLowerCase();

            let images = document.getElementById('gallery').getElementsByClassName('gallery-item');

            for (let i = 0; i < images.length; i++) {

                let captionText = images[i].getElementsByClassName('caption')[0].innerText.toLowerCase();

                if (captionText.includes(input)) {

                    images[i].style.display = "block";

                } else {

                    images[i].style.display = "none";

                }

            }

        }

        // إظهار حقل عنوان الصورة وزر الرفع بعد اختيار الصورة

        function showTitleInput() {

            document.getElementById('imageTitleInput').style.display = 'block';

            document.getElementById('submitButton').style.display = 'block';

        }

        // فتح الصورة في كامل الشاشة

        function openFullscreen(img) {

            let fullscreen = document.getElementById('fullscreen');

            let fullscreenImg = document.getElementById('fullscreenImg');

            fullscreenImg.src = img.src;

            fullscreen.style.display = "flex";

        }

        // إغلاق عرض الصورة في كامل الشاشة

        function closeFullscreen() {

            let fullscreen = document.getElementById('fullscreen');

            fullscreen.style.display = "none";

        }

        // حفظ الصورة

        function saveImage() {

            let fullscreenImg = document.getElementById('fullscreenImg');

            let link = document.createElement('a');

            link.href = fullscreenImg.src;

            link.download = "saved-image.jpg";

            link.click();

        }

        // رفع الصورة وإضافتها للمعرض مع العنوان

        function uploadImage() {

            let input = document.getElementById('uploadInput');

            let titleInput = document.getElementById('imageTitleInput');

            let gallery = document.getElementById('gallery');

            if (input.files && input.files[0]) {

                let reader = new FileReader();

                reader.onload = function(e) {

                    let newItem = document.createElement('div');

                    newItem.classList.add('gallery-item');

                    let newImage = document.createElement('img');

                    newImage.src = e.target.result;

                    newImage.alt = titleInput.value || "صورة جديدة";

                    newImage.onclick = function() { openFullscreen(newImage); };

                    let caption = document.createElement('div');

                    caption.classList.add('caption');

                    caption.innerText = titleInput.value || "عنوان الصورة";

                    newItem.appendChild(newImage);

                    newItem.appendChild(caption);

                    gallery.appendChild(newItem);

                    // حفظ الصور في LocalStorage

                    saveImages();

                    // إعادة تعيين حقول الإدخال

                    titleInput.value = '';

                    input.value = '';

                    // إخفاء حقل العنوان وزر الرفع بعد الانتهاء

                    titleInput.style.display = 'none';

                    document.getElementById('submitButton').style.display = 'none';

                };

                reader.readAsDataURL(input.files[0]);

            }

        }

        // حفظ الصور في LocalStorage

        function saveImages() {

            let galleryItems = document.getElementsByClassName('gallery-item');

            let images = [];

            for (let i = 0; i < galleryItems.length; i++) {

                let img = galleryItems[i].getElementsByTagName('img')[0];

                let caption = galleryItems[i].getElementsByClassName('caption')[0].innerText;

                images.push({

                    src: img.src,

                    caption: caption

                });

            }

            localStorage.setItem('images', JSON.stringify(images));

        }

        // تحميل الصور من LocalStorage عند تحميل الصفحة

        function loadImages() {

            let images = JSON.parse(localStorage.getItem('images')) || [];

            let gallery = document.getElementById('gallery');

            images.forEach(function(image) {

                let newItem = document.createElement('div');

                newItem.classList.add('gallery-item');

                let newImage = document.createElement('img');

                newImage.src = image.src;

                newImage.alt = image.caption;

                newImage.onclick = function() { openFullscreen(newImage); };

                let caption = document.createElement('div');

                caption.classList.add('caption');

                caption.innerText = image.caption;

                newItem.appendChild(newImage);

                newItem.appendChild(caption);

                gallery.appendChild(newItem);

            });

        }

        // تنفيذ حفظ الصور عند رفع صورة جديدة

        document.getElementById('submitButton').onclick = function() {

            uploadImage();

            saveImages();

        };

    </script>

</body>

</html>
