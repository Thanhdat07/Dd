<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3AE GROUP - Điểm Danh & Đớp</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { background-color: #121212; color: #ffffff; display: flex; justify-content: center; align-items: center; min-height: 100vh; overflow-x: hidden; }
        
        .container { background-color: #1e1e1e; padding: 30px; border-radius: 12px; box-shadow: 0 4px 30px rgba(0,0,0,0.7); width: 100%; max-width: 600px; text-align: center; margin: 20px; }
        h1 { color: #00e676; margin-bottom: 20px; font-size: 2.5rem; text-transform: uppercase; letter-spacing: 2px; }
        
        /* --- Animations --- */
        @keyframes fadeInScale {
            0% { opacity: 0; transform: scale(0.9) translateY(20px); }
            100% { opacity: 1; transform: scale(1) translateY(0); }
        }
        .animate-enter { animation: fadeInScale 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards; }

        /* Auth Screen */
        .auth-container { display: block; }
        .form-group { margin-bottom: 15px; text-align: left; }
        label { display: block; margin-bottom: 5px; color: #b3b3b3; font-weight: 500; }
        input, select { width: 100%; padding: 10px; border-radius: 6px; border: 1px solid #333; background-color: #2c2c2c; color: white; outline: none; transition: 0.3s; }
        input:focus, select:focus { border-color: #00e676; box-shadow: 0 0 5px rgba(0, 230, 118, 0.5); }
        
        button { background-color: #00e676; color: #121212; font-weight: bold; padding: 12px 20px; border: none; border-radius: 6px; cursor: pointer; width: 100%; font-size: 1.1rem; transition: all 0.3s; margin-top: 10px; }
        button:hover { background-color: #00c853; transform: translateY(-2px); box-shadow: 0 4px 10px rgba(0,200,83,0.4); }
        .toggle-auth { color: #00e676; cursor: pointer; text-decoration: underline; margin-top: 15px; display: inline-block; font-size: 0.9rem; }
        
        /* Avatar Selection */
        .avatar-selection { display: flex; gap: 12px; overflow-x: auto; padding: 10px 0; scrollbar-width: thin; scrollbar-color: #555 #1e1e1e; }
        .avatar-selection::-webkit-scrollbar { height: 6px; }
        .avatar-selection::-webkit-scrollbar-thumb { background: #555; border-radius: 3px; }
        .avatar-selection img { width: 60px; height: 60px; border-radius: 50%; object-fit: cover; cursor: pointer; border: 3px solid transparent; transition: all 0.3s ease; opacity: 0.6; }
        .avatar-selection img:hover { opacity: 0.9; transform: scale(1.05); }
        .avatar-selection img.selected { border-color: #00e676; transform: scale(1.1); box-shadow: 0 0 15px rgba(0, 230, 118, 0.6); opacity: 1; }

        /* Main Screen */
        .main-container { display: none; }
        .header { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #333; padding-bottom: 15px; margin-bottom: 20px; }
        
        .user-profile { display: flex; align-items: center; gap: 15px; }
        .user-profile img { width: 60px; height: 60px; border-radius: 50%; object-fit: cover; border: 2px solid #00e676; box-shadow: 0 0 10px rgba(0,230,118,0.3); }
        .user-info { text-align: left; }
        
        .balance-container { text-align: right; }
        .balance { font-size: 1.5rem; color: #ffd700; font-weight: bold; margin-bottom: 5px; }
        .logout-btn { background-color: #ff5252; width: auto; padding: 6px 12px; font-size: 0.85rem; margin: 0; color: white; border-radius: 4px; }
        .logout-btn:hover { background-color: #ff1744; box-shadow: 0 4px 10px rgba(255,82,82,0.4); }
        
        /* Dashboard Sections */
        .card { background-color: #2c2c2c; padding: 20px; border-radius: 10px; margin-bottom: 20px; border: 1px solid #444; transition: transform 0.3s; }
        .card:hover { transform: translateY(-5px); border-color: #555; }
        .card h2 { margin-bottom: 15px; color: #00e676; font-size: 1.5rem; }
        
        /* Điểm danh */
        .attendance-info { margin-bottom: 15px; font-size: 1.1rem; }
        .btn-attendance { background-color: #29b6f6; color: white; }
        .btn-attendance:hover:not(:disabled) { background-color: #03a9f4; box-shadow: 0 4px 10px rgba(41,182,246,0.4); }
        .btn-attendance:disabled { background-color: #444; cursor: not-allowed; color: #888; transform: none; box-shadow: none; }

        /* Đớp */
        .mouth-btn { font-size: 5.5rem; cursor: pointer; user-select: none; transition: transform 0.1s cubic-bezier(0.175, 0.885, 0.32, 1.275); display: inline-block; background: none; border: none; outline: none; margin: 10px 0; }
        .mouth-btn:active { transform: scale(0.75); }
        .click-count { font-size: 1.2rem; color: #ff4081; margin-top: 10px; font-weight: bold; }

        /* Leaderboard */
        .leaderboard { text-align: left; margin-top: 15px; }
        .leaderboard-item { display: flex; justify-content: space-between; align-items: center; padding: 12px 10px; border-bottom: 1px solid #444; transition: background 0.2s; border-radius: 6px; }
        .leaderboard-item:hover { background-color: #333; }
        .leaderboard-item:last-child { border-bottom: none; }
        
        .lb-user-info { display: flex; align-items: center; gap: 12px; }
        .lb-user-info img { width: 45px; height: 45px; border-radius: 50%; object-fit: cover; border: 1px solid #555; }
        .rank { font-weight: bold; color: #00e676; width: 25px; font-size: 1.1rem; }
        
        .error { color: #ff5252; margin-top: 10px; display: none; font-weight: bold; animation: fadeInScale 0.3s; }
    </style>
</head>
<body>

<div class="container animate-enter" id="main-wrapper">
    <h1>3AE GROUP</h1>
    
    <!-- Màn hình Đăng nhập / Đăng ký -->
    <div class="auth-container animate-enter" id="auth-screen">
        <h2 id="auth-title">Đăng Nhập</h2>
        
        <div class="form-group" id="avatar-group" style="display: none;">
            <label>Chọn Ảnh Đại Diện (Cầu thủ / Idol):</label>
            <div class="avatar-selection" id="avatar-list">
                <!-- Javascript sẽ render avatar vào đây -->
            </div>
        </div>

        <div class="form-group" id="role-group" style="display: none;">
            <label>Chức vụ:</label>
            <select id="role">
                <option value="Nhân viên">Nhân viên</option>
                <option value="Trưởng phòng">Trưởng phòng</option>
                <option value="Giám đốc">Giám đốc</option>
            </select>
        </div>

        <div class="form-group">
            <label>Tên người dùng:</label>
            <input type="text" id="username" placeholder="Nhập tên tài khoản" required>
        </div>
        <div class="form-group">
            <label>Mật khẩu:</label>
            <input type="password" id="password" placeholder="Nhập mật khẩu" required>
        </div>
        
        <button id="auth-btn" onclick="handleAuth()">Đăng Nhập</button>
        <p class="error" id="auth-error">Lỗi hiển thị ở đây</p>
        <span class="toggle-auth" onclick="toggleAuthMode()">Chưa có tài khoản? Đăng ký ngay</span>
    </div>

    <!-- Màn hình Chính -->
    <div class="main-container" id="main-screen">
        <div class="header">
            <div class="user-profile">
                <img id="display-avatar" src="" alt="Avatar">
                <div class="user-info">
                    <h3 id="display-name">Người dùng</h3>
                    <small id="display-role" style="color: #00e676;">Chức vụ</small>
                </div>
            </div>
            <div class="balance-container">
                <div class="balance" id="display-balance">0 VND</div>
                <button class="logout-btn" onclick="logout()">Đăng xuất</button>
            </div>
        </div>

        <!-- Section Điểm Danh -->
        <div class="card">
            <h2>Điểm Danh Hằng Tuần</h2>
            <div class="attendance-info" id="attendance-status">Hôm nay chưa điểm danh!</div>
            <p style="font-size: 0.9rem; color: #aaa; margin-bottom: 15px;">(Ngày 1: 1M. Các ngày sau nhân đôi. Bỏ lỡ trừ 500k/ngày)</p>
            <button class="btn-attendance" id="btn-attendance" onclick="markAttendance()">Điểm Danh Nhận Thưởng</button>
        </div>

        <!-- Section "Đớp" -->
        <div class="card">
            <h2>Chuyên Mục "ĐỚP"</h2>
            <p>Nhấp vào miệng để húp 1,000 VNĐ</p>
            <div class="mouth-btn" onclick="dop()">👄</div>
            <div class="click-count">Số lượt đớp: <strong id="display-clicks">0</strong></div>
        </div>

        <!-- Section Bảng Xếp Hạng -->
        <div class="card">
            <h2>Top "Hạm Đội Đớp"</h2>
            <div class="leaderboard" id="leaderboard-list">
                <!-- Data đổ vào đây -->
            </div>
        </div>
    </div>
</div>

<script>
    // Danh sách Avatar (Cầu thủ nổi tiếng & Idol nữ Tiktok/Kpop)
    const avatars = [
        "https://upload.wikimedia.org/wikipedia/commons/c/c1/Lionel_Messi_20180626.jpg", // Messi
        "https://upload.wikimedia.org/wikipedia/commons/8/8c/Cristiano_Ronaldo_2018.jpg", // Ronaldo
        "https://upload.wikimedia.org/wikipedia/commons/6/65/20180610_FIFA_Friendly_Match_Austria_vs._Brazil_Neymar_850_1705.jpg", // Neymar
        "https://upload.wikimedia.org/wikipedia/commons/3/30/190809_Jang_Won-young.jpg", // Wonyoung (Idol)
        "https://upload.wikimedia.org/wikipedia/commons/f/fc/Lisa_Blackpink_2019.jpg", // Lisa (Idol)
        "https://upload.wikimedia.org/wikipedia/commons/c/cc/IU_Melon_Music_Awards_2017.jpg" // IU (Idol)
    ];

    let selectedAvatar = avatars[0];

    // Khởi tạo Database tạm thời bằng LocalStorage
    let db = JSON.parse(localStorage.getItem('3ae_db')) || { users: {} };
    let currentUser = localStorage.getItem('3ae_currentUser') || null;
    let isRegisterMode = false;

    // Các biến DOM
    const authScreen = document.getElementById('auth-screen');
    const mainScreen = document.getElementById('main-screen');
    const authTitle = document.getElementById('auth-title');
    const roleGroup = document.getElementById('role-group');
    const avatarGroup = document.getElementById('avatar-group');
    const authBtn = document.getElementById('auth-btn');
    const toggleAuth = document.querySelector('.toggle-auth');
    const authError = document.getElementById('auth-error');

    // Hàm render danh sách Avatar lúc đăng ký
    function renderAvatars() {
        const list = document.getElementById('avatar-list');
        list.innerHTML = avatars.map((url, index) => `
            <img src="${url}" 
                 class="avatar-option ${index === 0 ? 'selected' : ''}" 
                 onclick="selectAvatar(this, '${url}')"
                 title="Avatar ${index + 1}">
        `).join('');
    }

    function selectAvatar(element, url) {
        document.querySelectorAll('.avatar-option').forEach(el => el.classList.remove('selected'));
        element.classList.add('selected');
        selectedAvatar = url;
    }

    // Các hàm tiện ích thời gian & tiền tệ
    function getTodayStr() { return new Date().toISOString().split('T')[0]; }
    function getDaysDiff(date1, date2) {
        const d1 = new Date(date1); const d2 = new Date(date2);
        return Math.floor((d2 - d1) / (1000 * 60 * 60 * 24));
    }
    function formatMoney(amount) { return amount.toLocaleString('vi-VN') + " VNĐ"; }

    // --- LOGIC GIAO DIỆN XÁC THỰC ---
    function triggerAnimation(element) {
        element.classList.remove('animate-enter');
        void element.offsetWidth; // Trigger reflow để reset animation
        element.classList.add('animate-enter');
    }

    function toggleAuthMode() {
        isRegisterMode = !isRegisterMode;
        authError.style.display = 'none';
        triggerAnimation(authScreen);

        if(isRegisterMode) {
            authTitle.innerText = "Đăng Ký Tài Khoản";
            roleGroup.style.display = "block";
            avatarGroup.style.display = "block";
            authBtn.innerText = "Đăng Ký";
            toggleAuth.innerText = "Đã có tài khoản? Đăng nhập";
            renderAvatars(); // Load avatar khi bấm sang đăng ký
        } else {
            authTitle.innerText = "Đăng Nhập";
            roleGroup.style.display = "none";
            avatarGroup.style.display = "none";
            authBtn.innerText = "Đăng Nhập";
            toggleAuth.innerText = "Chưa có tài khoản? Đăng ký ngay";
        }
    }

    function showError(msg) {
        authError.innerText = msg;
        authError.style.display = 'block';
        triggerAnimation(authError);
    }

    function handleAuth() {
        const user = document.getElementById('username').value.trim();
        const pass = document.getElementById('password').value.trim();
        const role = document.getElementById('role').value;

        if(!user || !pass) return showError("Vui lòng điền đủ thông tin!");

        if(isRegisterMode) {
            if(db.users[user]) return showError("Tên người dùng đã tồn tại! Chọn tên khác.");
            
            // Đăng ký user mới lưu kèm Avatar
            db.users[user] = {
                password: pass,
                role: role,
                avatar: selectedAvatar,
                balance: 0,
                clicks: 0,
                streak: 0, 
                lastActiveDate: getTodayStr(), 
                lastAttendanceDate: ""
            };
            saveDB();
            alert("Đăng ký thành công!");
            toggleAuthMode();
        } else {
            // Đăng nhập
            if(!db.users[user]) return showError("Tài khoản không tồn tại!");
            if(db.users[user].password !== pass) return showError("Sai mật khẩu!");
            
            currentUser = user;
            localStorage.setItem('3ae_currentUser', currentUser);
            
            applyPenaltyIfNeeded();
            loadMainScreen();
        }
    }

    function applyPenaltyIfNeeded() {
        let userData = db.users[currentUser];
        let today = getTodayStr();
        
        if (userData.lastActiveDate && userData.lastActiveDate !== today) {
            let daysMissed = getDaysDiff(userData.lastActiveDate, today);
            if (daysMissed > 1) {
                let actualMissed = daysMissed - 1;
                let penalty = actualMissed * 500000;
                userData.balance -= penalty;
                userData.streak = 0;
                alert(`CẢNH BÁO: Bạn đã không điểm danh ${actualMissed} ngày. Hệ thống đã trừ ${formatMoney(penalty)} trong tài khoản!`);
            }
        }
        userData.lastActiveDate = today;
        saveDB();
    }

    function logout() {
        currentUser = null;
        localStorage.removeItem('3ae_currentUser');
        document.getElementById('username').value = '';
        document.getElementById('password').value = '';
        
        mainScreen.style.display = 'none';
        authScreen.style.display = 'block';
        triggerAnimation(authScreen);
    }

    // --- LOGIC MÀN HÌNH CHÍNH ---
    function loadMainScreen() {
        authScreen.style.display = 'none';
        mainScreen.style.display = 'block';
        triggerAnimation(mainScreen);
        
        updateUI();
        updateLeaderboard();
    }

    function updateUI() {
        let userData = db.users[currentUser];
        document.getElementById('display-name').innerText = currentUser;
        document.getElementById('display-role').innerText = userData.role;
        document.getElementById('display-avatar').src = userData.avatar || avatars[0]; // Fallback avatar nếu tài khoản cũ chưa có
        document.getElementById('display-balance').innerText = formatMoney(userData.balance);
        document.getElementById('display-clicks').innerText = userData.clicks.toLocaleString('vi-VN');

        let btnAtt = document.getElementById('btn-attendance');
        let attStatus = document.getElementById('attendance-status');
        
        let today = getTodayStr();
        if(userData.lastAttendanceDate === today) {
            btnAtt.disabled = true;
            btnAtt.innerText = "Đã Điểm Danh Hôm Nay";
            attStatus.innerText = `Đang ở Ngày ${userData.streak}/7 của chuỗi.`;
        } else {
            btnAtt.disabled = false;
            let nextStreak = (userData.streak % 7) + 1;
            let nextReward = 1000000 * Math.pow(2, nextStreak - 1);
            btnAtt.innerText = `Điểm Danh (Nhận ${formatMoney(nextReward)})`;
            attStatus.innerHTML = `Bạn <span style="color:#ff5252; font-weight:bold;">chưa điểm danh</span> hôm nay!`;
        }
    }

    // --- CHỨC NĂNG ĐIỂM DANH ---
    function markAttendance() {
        let userData = db.users[currentUser];
        let today = getTodayStr();
        
        if(userData.lastAttendanceDate === today) return; 
        
        userData.streak = (userData.streak % 7) + 1; 
        let reward = 1000000 * Math.pow(2, userData.streak - 1); 
        
        userData.balance += reward;
        userData.lastAttendanceDate = today;
        
        saveDB();
        updateUI();
        
        alert(`Điểm danh thành công Ngày ${userData.streak}! Bạn nhận được ${formatMoney(reward)}`);
    }

    // --- CHỨC NĂNG ĐỚP (CLICKER) ---
    function dop() {
        let userData = db.users[currentUser];
        userData.clicks += 1;
        userData.balance += 1000;
        
        document.getElementById('display-balance').innerText = formatMoney(userData.balance);
        document.getElementById('display-clicks').innerText = userData.clicks.toLocaleString('vi-VN');
        
        saveDB();
        updateLeaderboard();
    }

    // --- BẢNG XẾP HẠNG ---
    function updateLeaderboard() {
        let listStr = '';
        
        let usersArray = Object.keys(db.users).map(username => {
            return {
                username: username,
                clicks: db.users[username].clicks,
                role: db.users[username].role,
                avatar: db.users[username].avatar || avatars[0] // Fallback avatar
            };
        });

        usersArray.sort((a, b) => b.clicks - a.clicks);
        let topUsers = usersArray.slice(0, 10);
        
        topUsers.forEach((u, index) => {
            listStr += `
                <div class="leaderboard-item">
                    <div class="lb-user-info">
                        <span class="rank">#${index + 1}</span> 
                        <img src="${u.avatar}" alt="Avatar">
                        <div>
                            <strong>${u.username}</strong> 
                            <div style="font-size: 0.8rem; color: #aaa;">${u.role}</div>
                        </div>
                    </div>
                    <div style="color: #ff4081; font-weight: bold; font-size: 1.1rem;">
                        ${u.clicks.toLocaleString('vi-VN')} Lượt
                    </div>
                </div>
            `;
        });

        document.getElementById('leaderboard-list').innerHTML = listStr;
    }

    function saveDB() {
        localStorage.setItem('3ae_db', JSON.stringify(db));
    }

    // Khởi chạy khi load trang
    window.onload = () => {
        if(currentUser && db.users[currentUser]) {
            applyPenaltyIfNeeded();
            loadMainScreen();
        } else {
            authScreen.style.display = 'block';
            mainScreen.style.display = 'none';
        }
    }
</script>

</body>
</html>

