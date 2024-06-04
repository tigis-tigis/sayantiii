import java.io.*;
import java.net.*;
import java.util.Base64;
import java.util.Scanner;

public class EmailClient {
    public static void main(String[] args) {
        // Define the SMTP server details
        String host = "smtp.gmail.com";
        int port = 587;

        // Get the username and password from the user
        Scanner scanner = new Scanner(System.in);
        System.out.println("Enter your email address:");
        String username = scanner.nextLine();
        System.out.println("Enter your password:");
        String password = scanner.nextLine();

        // Get the recipient's email address from the user
        System.out.println("Enter the recipient's email address:");
        String to = scanner.nextLine();

        // Get the email subject from the user
        System.out.println("Enter the email subject:");
        String subject = scanner.nextLine();

        // Get the email body from the user
        System.out.println("Enter the email body:");
        String body = scanner.nextLine();

        try {
            // Establish a connection to the SMTP server
            Socket socket = new Socket(host, port);
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));

            // Send the EHLO command to the SMTP server
            out.println("EHLO " + host);

            // Send the AUTH LOGIN command to the SMTP server
            out.println("AUTH LOGIN");

            // Send the username in Base64 format
            out.println(Base64.getEncoder().encodeToString(username.getBytes()));

            // Send the password in Base64 format
            out.println(Base64.getEncoder().encodeToString(password.getBytes()));

            // Send the MAIL FROM command to the SMTP server
            out.println("MAIL FROM: <" + username + ">");

            // Send the RCPT TO command to the SMTP server
            out.println("RCPT TO: <" + to + ">");

            // Send the DATA command to the SMTP server
            out.println("DATA");

            // Send the email subject
            out.println("Subject: " + subject);

            // Send the email body
            out.println(body);

            // Send a period to indicate the end of the email
            out.println(".");

            // Send the QUIT command to the SMTP server
            out.println("QUIT");

            // Close the socket
            socket.close();

            // Print a success message
            System.out.println("Mail successfully sent");

        } catch (IOException e) {
            // Handle any IO exceptions that occur
            e.printStackTrace();
        }
    }
}
