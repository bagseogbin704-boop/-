<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Taycan Style Bike Dashboard</title>
    <style>
        /* 타이칸 특유의 미니멀 다크 테마 */
        body {
            margin: 0;
            background-color: #050505;
            color: #ffffff;
            font-family: 'Segoe UI', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        #dashboard {
            text-align: center;
            width: 90%;
            max-width: 500px;
        }

        /* 중앙 속도 표시 */
        .speed-container {
            position: relative;
            padding: 40px;
        }

        #speed-value {
            font-size: 120px;
            font-weight: 200;
            margin: 0;
            letter-spacing: -5px;
        }

        .unit {
            font-size: 20px;
            color: #666;
            text-transform: uppercase;
        }

        /* 상태 바 (타이칸 레이아웃 참고) */
        .status-info {
            display: flex;
            justify-content: space-around;
            margin-top: 30px;
            color: #00e5ff; /* 일렉트릭 블루 포인트 */
            font-size: 14px;
        }

        .error-msg {
            color: #ff4b2b;
            font-size: 12px;
            margin-top: 10px;
        }
    </style>
</head>
<body>

<div id="dashboard">
    <div class="speed-container">
        <p id="speed-value">0</p>
        <span class="unit">km/h</span>
    </div>
    
    <div class="status-info">
        <div>DISTANCE: <span id="dist">0.0</span> km</div>
        <div>ACCURACY: <span id="acc">--</span> m</div>
    </div>
    <div id="error" class="error-msg"></div>
</div>

<script>
    const speedElement = document.getElementById('speed-value');
    const accElement = document.getElementById('acc');
    const errorElement = document.getElementById('error');

    // GPS 옵션 설정
    const options = {
        enableHighAccuracy: true, // 높은 정확도 모드 (배터리 소모 높음)
        maximumAge: 0,
        timeout: 5000
    };

    function updateDashboard(position) {
        // speed 값은 m/s 단위이므로 km/h로 변환 (m/s * 3.6)
        let speed = position.coords.speed;
        if (speed === null || speed < 0) speed = 0;
        
        const kmh = (speed * 3.6).toFixed(0);
        speedElement.innerText = kmh;
        
        // 정확도 표시
        accElement.innerText = position.coords.accuracy.toFixed(1);
        errorElement.innerText = "";
    }

    function handleError(error) {
        errorElement.innerText = "GPS 접근 오류: " + error.message;
    }

    // 위치 추적 시작
    if ("geolocation" in navigator) {
        navigator.geolocation.watchPosition(updateDashboard, handleError, options);
    } else {
        errorElement.innerText = "이 브라우저는 GPS를 지원하지 않습니다.";
    }
</script>

</body>
</html>
# -
