<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Scanner</title>
    <style>
    body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    background-color: #f5f5f5;
}

.card {
    text-align: center;
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h1 {
    margin-bottom: 20px;
}

#qr-result {
    margin-top: 20px;
    font-weight: bold;
}
        #video {
            width: 100%;
            max-width: 600px;
        }

        canvas {
            display: none;
        }
    </style>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous" />
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js" integrity="sha384-B4gt1jrGC7Jh4AgTPSdUtOBvfO8shuf57BaghqFfPlYxofvL8/KUEfYiJOMMV+rV" crossorigin="anonymous"></script>
</head>

<body>
    <div class="container mt-5">
        <h1 class="text-center"><b>WELCOME</b></h1>
        <div class="card">
            <h1>QR Scanner</h1>
            <div class="d-flex flex-row">
                <button class="mr-2 " onclick="openCamera()">Scan QR Code</button>
                <input type="file" accept="image/*" onchange="uploadQR(event)">
            </div>
        </div>
        <video id="video" playsinline></video>
        <canvas id="canvas"></canvas>
        <div id="qrResult"></div>

    </div>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@10"></script>
    <script src="https://rawgit.com/schmich/instascan-builds/master/instascan.min.js"></script>

    <script>
        // Initialize an empty set to store scanned QR data
        const scannedData = new Set();

        // Function to open camera and scan QR code
        function openCamera() {
            const video = document.getElementById('video');
            const canvas = document.getElementById('canvas');
            const qrResult = document.getElementById('qrResult');

            const scanner = new Instascan.Scanner({
                video: video
            });

            scanner.addListener('scan', function(content) {
                if (!scannedData.has(content)) {
                    scannedData.add(content);
                    displayQRData(content);
                } else {
                    Swal.fire('Error', 'This QR code has already been scanned.', 'error');
                }
            });

            Instascan.Camera.getCameras().then(function(cameras) {
                if (cameras.length > 0) {
                    scanner.start(cameras[0]);
                } else {
                    console.error('No cameras found.');
                    Swal.fire('Error', 'No cameras found.', 'error');
                }
            }).catch(function(error) {
                console.error('Error accessing camera:', error);
                Swal.fire('Error', 'Error accessing camera.', 'error');
            });
        }

        // Function to upload QR code image file
        function uploadQR(event) {
            const file = event.target.files[0];
            const reader = new FileReader();

            reader.onload = function(e) {
                const qrData = e.target.result;
                if (!scannedData.has(qrData)) {
                    scannedData.add(qrData);
                    displayQRData(qrData);
                } else {
                    Swal.fire('Error', 'This QR code has already been scanned.', 'error');
                }
            };

            reader.readAsDataURL(file);
        }

        // Function to display QR data
        function displayQRData(qrData) {
            const qrResult = document.getElementById('qrResult');
            qrResult.innerHTML += <p>${qrData}</p>;
        }
    </script>
</body>

</html>
