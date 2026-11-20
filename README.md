<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>AM EARN HUB</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>

    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', system-ui, -apple-system, sans-serif; user-select: none; }
        body {
            min-height: 100vh; background-color: #f4f8fb; overflow-x: hidden; position: relative;
            display: flex; flex-direction: column; align-items: center; padding: 14px 14px 104px 14px; color: #0f172a;
        }
        .light-bg-wrapper {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 0; overflow: hidden;
            background: linear-gradient(135deg, #ffffff 0%, #e6f4ff 50%, #f0f7ff 100%);
        }
        .glow-orb {
            position: absolute; border-radius: 50%; filter: blur(75px); opacity: 0.75;
            animation: floatLight 10s infinite alternate ease-in-out;
        }
        .orb-1 { width: 350px; height: 350px; background: #38bdf8; top: -80px; left: -80px; }
        .orb-2 { width: 400px; height: 400px; background: #67e8f9; bottom: -100px; right: -60px; animation-delay: -3s; }
        .orb-3 { width: 280px; height: 280px; background: #c084fc; top: 40%; left: 50%; transform: translate(-50%, -50%); opacity: 0.4; animation-delay: -6s; }
        .grid-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background-image: linear-gradient(rgba(42, 171, 238, 0.08) 1px, transparent 1px), linear-gradient(90deg, rgba(42, 171, 238, 0.08) 1px, transparent 1px);
            background-size: 30px 30px; z-index: 1; pointer-events: none;
        }
        @keyframes floatLight { 0% { transform: translate(0, 0) scale(1); } 100% { transform: translate(40px, 30px) scale(1.15); } }
        .app-wrapper { position: relative; z-index: 2; width: 100%; max-width: 420px; display: flex; flex-direction: column; gap: 14px; }
        .glass-card {
            background: rgba(255, 255, 255, 0.88); backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px);
            border: 1.5px solid rgba(255, 255, 255, 0.95); border-radius: 20px; padding: 18px;
            box-shadow: 0 12px 28px rgba(42, 171, 238, 0.09); text-align: center;
        }

        .profile-top-bar {
            display: flex; justify-content: space-between; align-items: center; gap: 8px; margin-bottom: 12px;
        }
        .bot-badge {
            display: inline-block; background: linear-gradient(135deg, #2aabee, #0088cc); color: #ffffff;
            padding: 4px 10px; border-radius: 50px; font-size: 10px; font-weight: 700; letter-spacing: 0.4px;
        }
        .country-select {
            padding: 5px 10px; border-radius: 12px; border: 1px solid rgba(2, 132, 199, 0.35);
            background: #ffffff; color: #0f172a; font-size: 11px; font-weight: 700; outline: none;
        }
        .profile-identity {
            display: flex; align-items: center; gap: 12px; text-align: left; margin-bottom: 14px;
            background: rgba(248, 250, 252, 0.9); border: 1px solid rgba(42, 171, 238, 0.2);
            border-radius: 16px; padding: 10px 12px;
        }
        .profile-avatar-wrap {
            width: 52px; height: 52px; border-radius: 50%; flex-shrink: 0;
            background: linear-gradient(135deg, #4f46e5, #0284c7); color: #ffffff;
            display: flex; align-items: center; justify-content: center;
            font-size: 20px; font-weight: 800; overflow: hidden;
            border: 2px solid #ffffff; box-shadow: 0 4px 12px rgba(2, 132, 199, 0.2);
        }
        .profile-avatar-wrap img { width: 100%; height: 100%; object-fit: cover; display: block; }
        .profile-name { font-size: 16px; font-weight: 800; color: #0f172a; line-height: 1.25; }
        .profile-id { font-size: 11px; font-weight: 700; color: #64748b; margin-top: 2px; }

        .profile-stats-grid {
            display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px;
        }
        .p-stat-box {
            background: #f0f9ff; border: 1px solid rgba(42, 171, 238, 0.25);
            border-radius: 14px; padding: 10px 6px; text-align: center;
        }
        .p-stat-label { font-size: 10px; font-weight: 700; color: #0369a1; }
        .p-stat-val { font-size: 15px; font-weight: 800; color: #0f172a; margin-top: 3px; word-break: break-word; }

        .balance-sub { font-size: 11px; font-weight: 700; color: #475569; margin-top: 8px; }
        .token-note-pill {
            display: none; margin-top: 6px; padding: 6px 10px; border-radius: 10px;
            background: #fef9c3; border: 1px solid #fde047; color: #854d0e;
            font-size: 11px; font-weight: 700; line-height: 1.4;
        }

        .checkin-overlay {
            position: fixed; inset: 0; background: rgba(15, 23, 42, 0.65); backdrop-filter: blur(4px);
            z-index: 60; display: none; align-items: center; justify-content: center; padding: 18px;
        }
        .checkin-modal {
            position: relative; width: 100%; max-width: 350px; background: #ffffff;
            border-radius: 22px; padding: 22px 18px; text-align: center;
            box-shadow: 0 22px 45px rgba(15, 23, 42, 0.28); border: 1.5px solid #e0f2fe;
            animation: fadeIn 0.2s ease;
        }
        .checkin-close {
            position: absolute; top: 12px; right: 12px; width: 32px; height: 32px;
            border-radius: 50%; border: 1px solid #e2e8f0; background: #f8fafc;
            color: #475569; font-size: 18px; font-weight: 800; cursor: pointer;
            display: flex; align-items: center; justify-content: center; line-height: 1;
        }
        .checkin-close:hover { background: #fee2e2; color: #b91c1c; border-color: #fecaca; }
        .checkin-badge {
            display: inline-block; background: #e0f2fe; color: #0369a1;
            padding: 4px 12px; border-radius: 20px; font-size: 11px; font-weight: 800; margin-bottom: 10px;
        }
        .checkin-title { font-size: 18px; font-weight: 800; color: #0f172a; margin-bottom: 6px; }
        .checkin-desc { font-size: 12px; color: #475569; margin-bottom: 14px; line-height: 1.5; }
        .checkin-reward-box {
            background: #f0f9ff; border: 1.5px dashed #38bdf8; border-radius: 16px;
            padding: 14px; margin-bottom: 14px; font-size: 20px; font-weight: 800; color: #0284c7;
        }

        .notice-banner {
            display: none; text-align: left; border-radius: 16px; padding: 12px 14px;
            border: 1.5px solid #bae6fd; background: rgba(240, 249, 255, 0.94); color: #0c4a6e;
            box-shadow: 0 8px 20px rgba(2, 132, 199, 0.08);
        }
        .notice-banner.work { background: #fffbeb; border-color: #fde68a; color: #78350f; }
        .notice-banner.issue { background: #fef2f2; border-color: #fecaca; color: #7f1d1d; }
        .notice-title { font-size: 13px; font-weight: 800; margin-bottom: 3px; }
        .notice-body { font-size: 12px; line-height: 1.5; }

        .section-title { font-size: 17px; font-weight: 700; color: #0f172a; margin-bottom: 6px; display: flex; align-items: center; justify-content: center; gap: 6px; }
        .section-desc { font-size: 13px; color: #475569; line-height: 1.5; margin-bottom: 14px; }
        .limit-note {
            background: #f0f9ff; border: 1px solid #bae6fd; color: #0369a1;
            border-radius: 12px; padding: 9px 12px; font-size: 12px; font-weight: 700; margin-bottom: 12px; line-height: 1.5;
        }
        .tax-preview-box {
            background: #fffbeb; border: 1px solid #fde68a; color: #92400e;
            border-radius: 12px; padding: 9px 12px; font-size: 12px; font-weight: 700; margin-bottom: 12px; line-height: 1.5; text-align: left;
        }
        .pause-alert {
            background: #fef2f2; border: 1.5px solid #fca5a5; color: #991b1b;
            border-radius: 14px; padding: 12px; font-size: 12px; font-weight: 600; margin-bottom: 14px;
            text-align: left; line-height: 1.6; display: none;
        }
        .pause-alert-title { font-size: 13px; font-weight: 800; color: #b91c1c; margin-bottom: 4px; }
        .block-overlay {
            position: fixed; inset: 0; background: rgba(15, 23, 42, 0.85); z-index: 70;
            display: none; align-items: center; justify-content: center; padding: 20px;
        }
        .block-card {
            background: #ffffff; border-radius: 22px; padding: 24px; max-width: 380px; width: 100%;
            text-align: center; box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
        }
        .block-card h2 { color: #b91c1c; font-size: 19px; margin-bottom: 8px; }
        .block-card p { color: #334155; font-size: 13px; line-height: 1.6; }

        .ads-grid, .tasks-grid, .games-picker-grid { display: flex; flex-direction: column; gap: 12px; }
        .ad-slot-card, .task-card, .game-picker-card {
            background: #ffffff;
            border: 1.5px solid rgba(2, 132, 199, 0.25);
            border-radius: 16px;
            padding: 14px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 12px;
            box-shadow: 0 6px 16px rgba(2, 132, 199, 0.06);
        }
        .ad-slot-info { text-align: left; flex: 1; }
        .ad-slot-title { font-size: 14px; font-weight: 800; color: #0f172a; }
        .ad-slot-reward { font-size: 12px; font-weight: 700; color: #0284c7; margin-top: 3px; }
        .ad-slot-limit { font-size: 11px; color: #64748b; margin-top: 2px; }
        .ad-watch-btn {
            padding: 10px 13px; border: none; border-radius: 12px;
            background: linear-gradient(135deg, #2aabee 0%, #0284c7 100%);
            color: #ffffff; font-size: 12px; font-weight: 700; cursor: pointer; white-space: nowrap;
        }
        .ad-watch-btn.claim-btn { background: linear-gradient(135deg, #16a34a 0%, #15803d 100%); }
        .ad-watch-btn:disabled { background: #94a3b8; cursor: not-allowed; }

        .primary-btn {
            width: 100%; padding: 14px; border: none; border-radius: 14px;
            background: linear-gradient(135deg, #2aabee 0%, #0284c7 100%);
            color: white; font-size: 15px; font-weight: 700; cursor: pointer;
        }
        .primary-btn:disabled { background: #94a3b8; cursor: not-allowed; }
        .secondary-back-btn {
            width: 100%; padding: 10px; border: 1px solid #cbd5e1; border-radius: 12px;
            background: #f8fafc; color: #0f172a; font-size: 13px; font-weight: 700;
            cursor: pointer; margin-bottom: 12px;
        }
        .game-btn { background: linear-gradient(135deg, #f59e0b 0%, #d97706 100%); margin-top: 12px; }
        .game-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; }
        .score-pill { background: #e0f2fe; color: #0369a1; padding: 4px 10px; border-radius: 20px; font-size: 12px; font-weight: 700; }
        #gameCanvas { width: 100%; height: 160px; background: linear-gradient(180deg, #e0f2fe 0%, #f8fafc 100%); border: 1.5px solid rgba(42, 171, 238, 0.3); border-radius: 14px; display: block; }
        #customGameHost { width: 100%; min-height: 220px; border-radius: 14px; overflow: hidden; background: #ffffff; border: 1px solid rgba(42, 171, 238, 0.25); }
        #customGameHost iframe { width: 100%; min-height: 360px; border: none; display: block; }
        .game-closed-box {
            display: none; background: #fef2f2; border: 1.5px solid #fca5a5; color: #991b1b;
            border-radius: 16px; padding: 16px; text-align: left; line-height: 1.6;
        }
        .game-closed-title { font-size: 14px; font-weight: 800; color: #b91c1c; margin-bottom: 6px; }
        .game-closed-reason { font-size: 12px; font-weight: 600; }
        .input-group { text-align: left; margin-bottom: 12px; }
        .input-group label { display: block; font-size: 12px; font-weight: 600; color: #334155; margin-bottom: 4px; }
        .form-control { width: 100%; padding: 12px; border-radius: 12px; border: 1px solid rgba(42, 171, 238, 0.3); background: rgba(255, 255, 255, 0.95); font-size: 14px; color: #0f172a; outline: none; }

        .history-box { margin-top: 18px; padding-top: 14px; border-top: 1px solid #e2e8f0; text-align: left; }
        .history-title { font-size: 14px; font-weight: 800; color: #0f172a; margin-bottom: 10px; }
        .history-item {
            background: rgba(248, 250, 252, 0.95); border: 1px solid rgba(42, 171, 238, 0.2);
            border-radius: 14px; padding: 11px 12px; margin-bottom: 8px; font-size: 12px; line-height: 1.5;
        }
        .history-top { display: flex; justify-content: space-between; align-items: center; gap: 8px; margin-bottom: 4px; font-weight: 700; }
        .st-badge { padding: 2px 8px; border-radius: 10px; font-size: 10px; font-weight: 800; }
        .st-pending { background: #fef3c7; color: #92400e; }
        .st-approved { background: #dcfce7; color: #166534; }
        .st-rejected { background: #fee2e2; color: #991b1b; }
        .reject-reason-box {
            margin-top: 5px; padding: 6px 8px; border-radius: 8px;
            background: #fef2f2; border: 1px solid #fecaca; color: #991b1b; font-size: 11px; font-weight: 600;
        }

        .lb-switch {
            display: grid; grid-template-columns: 1fr 1fr; gap: 8px;
            background: #e2e8f0; padding: 5px; border-radius: 14px; margin-bottom: 14px;
        }
        .lb-sub-btn {
            border: none; background: transparent; padding: 9px 8px; border-radius: 10px;
            font-size: 12px; font-weight: 700; color: #475569; cursor: pointer; transition: all 0.2s ease;
        }
        .lb-sub-btn.active {
            background: #ffffff; color: #0284c7; box-shadow: 0 3px 10px rgba(2, 132, 199, 0.14);
        }
        .lb-list { display: flex; flex-direction: column; gap: 8px; text-align: left; }
        .lb-row {
            display: flex; justify-content: space-between; align-items: center;
            background: rgba(255, 255, 255, 0.9); border: 1px solid rgba(42, 171, 238, 0.18);
            border-radius: 14px; padding: 11px 14px;
        }
        .lb-left { display: flex; align-items: center; gap: 10px; }
        .lb-rank {
            width: 28px; height: 28px; border-radius: 50%; display: flex; align-items: center; justify-content: center;
            font-size: 12px; font-weight: 800; background: #e0f2fe; color: #0369a1;
        }
        .lb-name { font-size: 13px; font-weight: 700; color: #0f172a; }
        .lb-val { font-size: 13px; font-weight: 800; color: #0284c7; white-space: nowrap; }

        .support-box {
            background: #f0f9ff; border: 1.5px solid #bae6fd; border-radius: 16px;
            padding: 16px; margin-bottom: 14px; text-align: center;
        }
        .support-target-text {
            font-size: 13px; font-weight: 800; color: #0369a1; margin-bottom: 10px; word-break: break-all;
        }

        .tab-section { display: none; }
        .tab-section.active { display: block; animation: fadeIn 0.25s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: translateY(0); } }

        .bottom-nav {
            position: fixed; bottom: 16px; left: 50%; transform: translateX(-50%); width: calc(100% - 20px); max-width: 420px;
            background: rgba(255, 255, 255, 0.96); backdrop-filter: blur(16px); border: 1.5px solid rgba(255, 255, 255, 1);
            border-radius: 20px; padding: 6px 3px; display: grid; grid-template-columns: repeat(7, 1fr); gap: 2px; z-index: 10;
            box-shadow: 0 8px 24px rgba(15, 23, 42, 0.12);
        }
        .nav-item { border: none; background: transparent; padding: 8px 1px; border-radius: 12px; font-size: 9.5px; font-weight: 700; color: #64748b; cursor: pointer; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .nav-item.active { background: linear-gradient(135deg, #2aabee, #0284c7); color: #ffffff; }
    </style>
</head>
<body>
    <div class="checkin-overlay" id="dailyCheckinOverlay">
        <div class="checkin-modal">
            <button type="button" class="checkin-close" id="btnCloseCheckin" onclick="window.closeDailyCheckin()" aria-label="Close">×</button>
            <div class="checkin-badge" id="lblCheckinBadge">📅 ডেইলি চেক-ইন</div>
            <div class="checkin-title" id="checkinModalTitle">আজকের ডেইলি চেক-ইন</div>
            <div class="checkin-desc" id="checkinModalDesc">আজকের বোনাস পয়েন্ট ক্লেম করুন অথবা ক্রস (×) চেপে বন্ধ করুন।</div>
            <div class="checkin-reward-box" id="checkinRewardDisplay">+20 pts</div>
            <button type="button" class="primary-btn" id="btnClaimCheckin" onclick="window.claimDailyCheckin()">🎁 এখনই ক্লেম করুন</button>
        </div>
    </div>

    <div class="block-overlay" id="userBlockOverlay">
        <div class="block-card">
            <h2 id="lblBlockedTitle">🚫 অ্যাকাউন্ট ব্লক করা হয়েছে</h2>
            <p id="userBlockReasonText">অ্যাডমিন আপনার অ্যাকাউন্ট সাময়িকভাবে বা স্থায়ীভাবে ব্লক করেছেন।</p>
        </div>
    </div>

    <div class="light-bg-wrapper">
        <div class="glow-orb orb-1"></div>
        <div class="glow-orb orb-2"></div>
        <div class="glow-orb orb-3"></div>
    </div>
    <div class="grid-overlay"></div>

    <div class="app-wrapper">
        <div class="notice-banner" id="appNoticeBanner">
            <div class="notice-title" id="appNoticeTitle">নোটিশ</div>
            <div class="notice-body" id="appNoticeBody"></div>
        </div>

        <div class="glass-card">
            <div class="profile-top-bar">
                <div class="bot-badge" id="lblBotBadge">● BOT ACTIVE</div>
                <select class="country-select" id="countrySelect" onchange="window.onCountryChanged()">
                    <option value="BD">🇧🇩 Bangladesh</option>
                    <option value="US">🇺🇸 United States</option>
                    <option value="IN">🇮🇳 India</option>
                    <option value="OTHER">🌐 Global</option>
                </select>
            </div>

            <div class="profile-identity">
                <div class="profile-avatar-wrap" id="profileAvatarBox">U</div>
                <div>
                    <div class="profile-name" id="userName">User</div>
                    <div class="profile-id">ID: <span id="userIdDisplay">guest_am_1332</span></div>
                </div>
            </div>

            <div class="profile-stats-grid">
                <div class="p-stat-box">
                    <div class="p-stat-label" id="lblStatBalance">ব্যালেন্স</div>
                    <div class="p-stat-val" id="coinBalance">120 pts</div>
                </div>
                <div class="p-stat-box">
                    <div class="p-stat-label" id="lblStatAds">মোট অ্যাড</div>
                    <div class="p-stat-val" id="statAdsWatched">0</div>
                </div>
                <div class="p-stat-box">
                    <div class="p-stat-label" id="lblStatRefer">রেফার</div>
                    <div class="p-stat-val" id="statReferCount">0</div>
                </div>
            </div>

            <div class="balance-sub" id="pointsSubInfo"></div>
            <div class="token-note-pill" id="tokenNoteDisplay"></div>
        </div>

        <!-- ১. অ্যাড জোন ট্যাব -->
        <div id="tab-ads" class="tab-section active">
            <div class="glass-card">
                <h2 class="section-title" id="lblAdsTitle">📺 অ্যাড জোন</h2>
                <p class="section-desc" id="lblAdsDesc">পুরো সময় অ্যাড দেখলে তবেই পয়েন্ট যোগ হবে; আগে বের হলে কাউন্ট হবে না।</p>
                <div class="ads-grid" id="adsContainer"></div>
            </div>
        </div>

        <!-- ২. টাস্ক ট্যাব -->
        <div id="tab-tasks" class="tab-section">
            <div class="glass-car
