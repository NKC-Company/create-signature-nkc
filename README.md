function handleCredentialResponse(response) {
const payload = parseJwt(response.credential);
const email = payload.email ?? "";

    // ✅ Cho phép login với các domain sau
    const allowedDomains = ["@nkcmc.com.vn", "@nkc-company.com"];
    const isAllowed = allowedDomains.some(domain => email.endsWith(domain));

    if (!isAllowed) {
        alert("Chỉ nhân sự công ty mới được phép truy cập.");
        // Xoá thông tin đăng nhập nếu cần
        google.accounts.id.disableAutoSelect();
        return;
    }

    // ✅ Nếu email hợp lệ, tiếp tục hiển thị form
    document.getElementById('loginSection').style.display = 'none';
    document.querySelector('.container-login').style.display = 'none';
    document.querySelector('.container').style.display = 'block';

    document.getElementById('name').value = payload.given_name ?? "";
    document.getElementById('title').value = "";
    document.getElementById('phone').value = "";
    document.getElementById('email').value = email;
    document.getElementById('website').value = "https://nkmc.com.vn";

}

if (!isAllowed) {
alert(`Email ${email} không được phép truy cập công cụ này.`);
google.accounts.id.disableAutoSelect();
return;
}
