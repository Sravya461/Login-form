<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Advanced Login & Signup</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Montserrat', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
        }

        body {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(135deg, #E8EAF6, #F5F5F5);
            padding: 20px;
            transition: background 0.3s;
        }

        body.dark-mode {
            background: linear-gradient(135deg, #1C2526, #2E3B3E);
            color: #E0E0E0;
        }

        .container {
            max-width: 480px;
            width: 100%;
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            position: relative;
            transition: background 0.3s, color 0.3s;
        }

        .dark-mode .container {
            background: rgba(33, 33, 33, 0.95);
            color: #E0E0E0;
        }

        .tabs {
            display: flex;
            background: #3F51B5;
        }

        .tab {
            flex: 1;
            padding: 16px;
            text-align: center;
            color: #FFFFFF;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.3s;
        }

        .tab.active {
            background: #FFB300;
        }

        .tab:hover {
            background: #303F9F;
        }

        .form-box {
            padding: 32px;
            transition: opacity 0.3s ease, transform 0.3s ease;
        }

        .form-box.hidden {
            opacity: 0;
            transform: translateY(10px);
            display: none;
        }

        .form-box h2 {
            font-size: 1.5rem;
            font-weight: 600;
            color: #212121;
            text-align: center;
            margin-bottom: 24px;
        }

        .dark-mode .form-box h2 {
            color: #E0E0E0;
        }

        .input-group {
            margin-bottom: 20px;
            position: relative;
        }

        .input-group input, .input-group checkbox {
            width: 100%;
            padding: 12px 40px 12px 16px;
            font-size: 0.95rem;
            border: 1px solid #B0BEC5;
            border-radius: 8px;
            outline: none;
            transition: border-color 0.2s, box-shadow 0.2s;
            background: #FFFFFF;
        }

        .dark-mode .input-group input {
            background: #424242;
            border-color: #616161;
            color: #E0E0E0;
        }

        .input-group input:focus {
            border-color: #FFB300;
            box-shadow: 0 0 0 3px rgba(255, 179, 0, 0.1);
        }

        .input-group label {
            position: absolute;
            top: 50%;
            left: 16px;
            transform: translateY(-50%);
            font-size: 0.95rem;
            color: #757575;
            pointer-events: none;
            transition: all 0.2s;
        }

        .dark-mode .input-group label {
            color: #B0BEC5;
        }

        .input-group input:focus + label,
        .input-group input:not(:placeholder-shown) + label {
            top: -8px;
            left: 12px;
            font-size: 0.75rem;
            background: #FFFFFF;
            padding: 0 4px;
            color: #FFB300;
        }

        .dark-mode .input-group input:focus + label,
        .dark-mode .input-group input:not(:placeholder-shown) + label {
            background: #424242;
        }

        .input-group .toggle-password {
            position: absolute;
            right: 16px;
            top: 50%;
            transform: translateY(-50%);
            cursor: pointer;
            color: #757575;
        }

        .dark-mode .input-group .toggle-password {
            color: #B0BEC5;
        }

        .error {
            color: #F44336;
            font-size: 0.75rem;
            margin-top: 4px;
            display: none;
        }

        .password-strength {
            font-size: 0.75rem;
            margin-top: 4px;
            color: #757575;
            display: none;
        }

        .password-strength.weak { color: #F44336; }
        .password-strength.medium { color: #FFB300; }
        .password-strength.strong { color: #4CAF50; }

        .checkbox-group {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            font-size: 0.85rem;
            color: #212121;
        }

        .dark-mode .checkbox-group {
            color: #E0E0E0;
        }

        .checkbox-group input {
            width: auto;
            margin-right: 8px;
        }

        .btn {
            width: 100%;
            padding: 12px;
            background: #FFB300;
            border: none;
            border-radius: 8px;
            color: #212121;
            font-size: 0.95rem;
            font-weight: 500;
            cursor: pointer;
            transition: background 0.2s;
            position: relative;
        }

        .btn:hover {
            background: #FFA000;
        }

        .btn.loading::after {
            content: '';
            position: absolute;
            width: 16px;
            height: 16px;
            border: 2px solid #212121;
            border-top: 2px solid transparent;
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
            right: 16px;
            top: 50%;
            transform: translateY(-50%);
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        .social-login {
            margin-top: 20px;
            text-align: center;
        }

        .social-login p {
            font-size: 0.85rem;
            color: #757575;
            margin-bottom: 12px;
        }

        .dark-mode .social-login p {
            color: #B0BEC5;
        }

        .social-btn {
            width: 100%;
            padding: 10px;
            margin-bottom: 8px;
            border: 1px solid #B0BEC5;
            border-radius: 8px;
            background: #FFFFFF;
            font-size: 0.9rem;
            cursor: pointer;
            transition: background 0.2s;
        }

        .dark-mode .social-btn {
            background: #424242;
            border-color: #616161;
            color: #E0E0E0;
        }

        .social-btn:hover {
            background: #ECEFF1;
        }

        .dark-mode .social-btn:hover {
            background: #616161;
        }

        .forgot-password {
            text-align: right;
            margin-bottom: 16px;
            font-size: 0.85rem;
        }

        .forgot-password a {
            color: #FFB300;
            text-decoration: none;
        }

        .forgot-password a:hover {
            text-decoration: underline;
        }

        .success-message {
            display: none;
            max-width: 480px;
            width: 100%;
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            padding: 32px;
            text-align: center;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            animation: slideIn 0.3s ease;
        }

        .dark-mode .success-message {
            background: rgba(33, 33, 33, 0.95);
            color: #E0E0E0;
        }

        @keyframes slideIn {
            from { transform: translate(-50%, -60%); opacity: 0; }
            to { transform: translate(-50%, -50%); opacity: 1; }
        }

        .success-message h2 {
            font-size: 1.5rem;
            font-weight: 600;
            color: #212121;
            margin-bottom: 12px;
        }

        .dark-mode .success-message h2 {
            color: #E0E0E0;
        }

        .success-message p {
            font-size: 0.95rem;
            color: #757575;
            margin-bottom: 16px;
        }

        .dark-mode .success-message p {
            color: #B0BEC5;
        }

        .success-message a {
            color: #FFB300;
            text-decoration: none;
            font-weight: 500;
        }

        .success-message a:hover {
            text-decoration: underline;
        }

        .modal {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            max-width: 400px;
            width: 90%;
            background: #FFFFFF;
            border-radius: 12px;
            padding: 24px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
            z-index: 1000;
        }

        .dark-mode .modal {
            background: #424242;
            color: #E0E0E0;
        }

        .modal h3 {
            font-size: 1.3rem;
            margin-bottom: 16px;
            color: #212121;
        }

        .dark-mode .modal h3 {
            color: #E0E0E0;
        }

        .modal .input-group {
            margin-bottom: 16px;
        }

        .modal .btn {
            margin-top: 16px;
        }

        .modal .close {
            position: absolute;
            top: 16px;
            right: 16px;
            font-size: 1.2rem;
            cursor: pointer;
            color: #757575;
        }

        .dark-mode .modal .close {
            color: #B0BEC5;
        }

        .dark-mode-toggle {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 8px 16px;
            background: #3F51B5;
            color: #FFFFFF;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 0.85rem;
        }

        .dark-mode .dark-mode-toggle {
            background: #FFB300;
            color: #212121;
        }

        @media (max-width: 480px) {
            .container, .success-message {
                max-width: 90%;
            }

            .form-box {
                padding: 24px;
            }

            .tabs {
                flex-direction: column;
            }

            .tab {
                padding: 12px;
            }
        }
    </style>
</head>
<body>
    <button class="dark-mode-toggle" onclick="toggleDarkMode()">Toggle Dark Mode</button>
    <div class="container">
        <div class="tabs">
            <div class="tab active" onclick="showLogin()">Login</div>
            <div class="tab" onclick="showSignup()">Sign Up</div>
        </div>
        <div class="form-box" id="login-form-box">
            <h2>Login</h2>
            <form id="login-form" onsubmit="return validateLogin(event)">
                <div class="input-group">
                    <input type="email" id="login-email" placeholder=" " required aria-label="Email">
                    <label for="login-email">Email</label>
                    <div class="error" id="login-email-error">Enter a valid email address.</div>
                </div>
                <div class="input-group">
                    <input type="password" id="login-password" placeholder=" " required aria-label="Password">
                    <label for="login-password">Password</label>
                    <span class="toggle-password" onclick="togglePassword('login-password')">👁️</span>
                    <div class="error" id="login-password-error">Password must be 8+ characters.</div>
                </div>
                <div class="checkbox-group">
                    <input type="checkbox" id="remember-me" aria-label="Remember Me">
                    <label for="remember-me">Remember Me</label>
                </div>
                <div class="forgot-password">
                    <a href="#" onclick="showForgotPassword()">Forgot Password?</a>
                </div>
                <button type="submit" class="btn">Login</button>
                <div class="social-login">
                    <p>Or login with</p>
                    <button type="button" class="social-btn">Google</button>
                    <button type="button" class="social-btn">GitHub</button>
                </div>
            </form>
        </div>
        <div class="form-box hidden" id="signup-form-box">
            <h2>Sign Up</h2>
            <form id="signup-form" onsubmit="return validateSignup(event)">
                <div class="input-group">
                    <input type="text" id="signup-username" placeholder=" " required aria-label="Username">
                    <label for="signup-username">Username</label>
                    <div class="error" id="signup-username-error">Username must be 4+ characters.</div>
                </div>
                <div class="input-group">
                    <input type="email" id="signup-email" placeholder=" " required aria-label="Email">
                    <label for="signup-email">Email</label>
                    <div class="error" id="signup-email-error">Enter a valid email address.</div>
                </div>
                <div class="input-group">
                    <input type="tel" id="signup-phone" placeholder=" " aria-label="Phone Number">
                    <label for="signup-phone">Phone Number (Optional)</label>
                    <div class="error" id="signup-phone-error">Enter a valid phone number.</div>
                </div>
                <div class="input-group">
                    <input type="password" id="signup-password" placeholder=" " required aria-label="Password" oninput="checkPasswordStrength()">
                    <label for="signup-password">Password</label>
                    <span class="toggle-password" onclick="togglePassword('signup-password')">👁️</span>
                    <div class="error" id="signup-password-error">Password must be 8+ characters.</div>
                    <div class="password-strength" id="password-strength"></div>
                </div>
                <div class="input-group">
                    <input type="password" id="signup-confirm-password" placeholder=" " required aria-label="Confirm Password">
                    <label for="signup-confirm-password">Confirm Password</label>
                    <span class="toggle-password" onclick="togglePassword('signup-confirm-password')">👁️</span>
                    <div class="error" id="signup-confirm-password-error">Passwords do not match.</div>
                </div>
                <div class="checkbox-group">
                    <input type="checkbox" id="terms" required aria-label="Accept Terms">
                    <label for="terms">I agree to the <a href="#" style="color: #FFB300;">Terms & Conditions</a></label>
                </div>
                <button type="submit" class="btn">Sign Up</button>
                <div class="social-login">
                    <p>Or sign up with</p>
                    <button type="button" class="social-btn">Google</button>
                    <button type="button" class="social-btn">GitHub</button>
                </div>
            </form>
        </div>
    </div>
    <div class="success-message" id="success-message">
        <h2>Welcome!</h2>
        <p>You’ve successfully logged in to your account.</p>
        <a href="#" onclick="showLogin()">Return to Login</a>
    </div>
    <div class="modal" id="forgot-password-modal">
        <span class="close" onclick="closeModal()">×</span>
        <h3>Reset Password</h3>
        <form id="reset-form" onsubmit="return validateReset(event)">
            <div class="input-group">
                <input type="email" id="reset-email" placeholder=" " required aria-label="Email">
                <label for="reset-email">Email</label>
                <div class="error" id="reset-email-error">Enter a valid email address.</div>
            </div>
            <button type="submit" class="btn">Send Reset Link</button>
        </form>
    </div>

    <script>
        function togglePassword(inputId) {
            const input = document.getElementById(inputId);
            const icon = input.nextElementSibling;
            if (input.type === 'password') {
                input.type = 'text';
                icon.textContent = '🙈';
            } else {
                input.type = 'password';
                icon.textContent = '👁️';
            }
        }

        function toggleDarkMode() {
            document.body.classList.toggle('dark-mode');
        }

        function showLogin() {
            document.getElementById('login-form-box').classList.remove('hidden');
            document.getElementById('signup-form-box').classList.add('hidden');
            document.getElementById('success-message').style.display = 'none';
            document.querySelectorAll('.tab')[0].classList.add('active');
            document.querySelectorAll('.tab')[1].classList.remove('active');
        }

        function showSignup() {
            document.getElementById('signup-form-box').classList.remove('hidden');
            document.getElementById('login-form-box').classList.add('hidden');
            document.getElementById('success-message').style.display = 'none';
            document.querySelectorAll('.tab')[1].classList.add('active');
            document.querySelectorAll('.tab')[0].classList.remove('active');
        }

        function showSuccessMessage() {
            document.querySelector('.container').style.display = 'none';
            document.getElementById('success-message').style.display = 'block';
        }

        function showForgotPassword() {
            document.getElementById('forgot-password-modal').style.display = 'block';
        }

        function closeModal() {
            document.getElementById('forgot-password-modal').style.display = 'none';
        }

        function checkPasswordStrength() {
            const password = document.getElementById('signup-password').value;
            const strength = document.getElementById('password-strength');
            strength.style.display = 'block';
            if (password.length < 8) {
                strength.textContent = 'Weak';
                strength.className = 'password-strength weak';
            } else if (password.length < 12 || !/[A-Z]/.test(password) || !/[0-9]/.test(password)) {
                strength.textContent = 'Medium';
                strength.className = 'password-strength medium';
            } else {
                strength.textContent = 'Strong';
                strength.className = 'password-strength strong';
            }
        }

        function validateLogin(event) {
            event.preventDefault();
            const email = document.getElementById('login-email').value;
            const password = document.getElementById('login-password').value;
            let isValid = true;

            document.getElementById('login-email-error').style.display = 'none';
            document.getElementById('login-password-error').style.display = 'none';
            const btn = event.target.querySelector('.btn');
            btn.classList.add('loading');
            btn.disabled = true;

            if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
                document.getElementById('login-email-error').style.display = 'block';
                isValid = false;
            }

            if (password.length < 8) {
                document.getElementById('login-password-error').style.display = 'block';
                isValid = false;
            }

            setTimeout(() => {
                btn.classList.remove('loading');
                btn.disabled = false;
                if (isValid) {
                    showSuccessMessage();
                }
            }, 1000);
            return false;
        }

        function validateSignup(event) {
            event.preventDefault();
            const username = document.getElementById('signup-username').value;
            const email = document.getElementById('signup-email').value;
            const phone = document.getElementById('signup-phone').value;
            const password = document.getElementById('signup-password').value;
            const confirmPassword = document.getElementById('signup-confirm-password').value;
            const terms = document.getElementById('terms').checked;
            let isValid = true;

            document.getElementById('signup-username-error').style.display = 'none';
            document.getElementById('signup-email-error').style.display = 'none';
            document.getElementById('signup-phone-error').style.display = 'none';
            document.getElementById('signup-password-error').style.display = 'none';
            document.getElementById('signup-confirm-password-error').style.display = 'none';
            const btn = event.target.querySelector('.btn');
            btn.classList.add('loading');
            btn.disabled = true;

            if (username.length < 4) {
                document.getElementById('signup-username-error').style.display = 'block';
                isValid = false;
            }

            if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
                document.getElementById('signup-email-error').style.display = 'block';
                isValid = false;
            }

            if (phone && !/^\+?\d{10,15}$/.test(phone)) {
                document.getElementById('signup-phone-error').style.display = 'block';
                isValid = false;
            }

            if (password.length < 8) {
                document.getElementById('signup-password-error').style.display = 'block';
                isValid = false;
            }

            if (password !== confirmPassword) {
                document.getElementById('signup-confirm-password-error').style.display = 'block';
                isValid = false;
            }

            if (!terms) {
                alert('Please accept the Terms & Conditions.');
                isValid = false;
            }

            setTimeout(() => {
                btn.classList.remove('loading');
                btn.disabled = false;
                if (isValid) {
                    alert('Account created successfully! (Demo mode)');
                    showLogin();
                }
            }, 1000);
            return false;
        }

        function validateReset(event) {
            event.preventDefault();
            const email = document.getElementById('reset-email').value;
            let isValid = true;

            document.getElementById('reset-email-error').style.display = 'none';
            const btn = event.target.querySelector('.btn');
            btn.classList.add('loading');
            btn.disabled = true;

            if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
                document.getElementById('reset-email-error').style.display = 'block';
                isValid = false;
            }

            setTimeout(() => {
                btn.classList.remove('loading');
                btn.disabled = false;
                if (isValid) {
                    alert('Password reset link sent! (Demo mode)');
                    closeModal();
                }
            }, 1000);
            return false;
        }
    </script>
</body>
</html>
