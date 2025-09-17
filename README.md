<?php
use PHPMailer\PHPMailer\PHPMailer;
use PHPMailer\PHPMailer\Exception;
require 'vendor/autoload.php';

$message = "";

if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $name = htmlspecialchars($_POST['name']);
    $email = filter_var($_POST['email'], FILTER_VALIDATE_EMAIL);
    $msg = htmlspecialchars($_POST['message']);

    if ($email) {
        $mail = new PHPMailer(true);
        try {
            $mail->isSMTP();
            $mail->Host = 'smtp.example.com';
            $mail->SMTPAuth = true;
            $mail->Username = 'your_email@example.com';
            $mail->Password = 'your_password';
            $mail->SMTPSecure = 'tls';
            $mail->Port = 587;

            $mail->setFrom($email, $name);
            $mail->addAddress('your_email@example.com');
            $mail->isHTML(true);
            $mail->Subject = "Contact Form Submission";
            $mail->Body = "<b>Name:</b> $name <br><b>Email:</b> $email <br><b>Message:</b> $msg";

            $mail->send();
            $message = "✅ Message sent successfully!";
        } catch (Exception $e) {
            $message = "❌ Error sending message: {$mail->ErrorInfo}";
        }
    } else {
        $message = "❌ Invalid email!";
    }
}
?>
<!DOCTYPE html>
<html>
<head><title>Professional Contact Form</title></head>
<body>
<h2>Contact Me</h2>
<form method="post">
    <input type="text" name="name" placeholder="Your Name" required><br>
    <input type="email" name="email" placeholder="Your Email" required><br>
    <textarea name="message" placeholder="Message..." required></textarea><br>
    <button type="submit">Send</button>
</form>
<p><?= $message ?></p>
</body>
</html>
