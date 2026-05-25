package com.mycompany.quickchat;

import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Scanner;

public class QuickChat {
    
    static class Message {
        String messageId;
        String recipient;
        String content;
        String hash;
        
        Message(String id, String recipient, String content, String hash) {
            this.messageId = id;
            this.recipient = recipient;
            this.content = content;
            this.hash = hash;
        }
    }
    
    static ArrayList<Message> messages = new ArrayList<>();
    static int messageCount = 0;
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        System.out.println("=== Welcome to QuickChat ===");
        
        // Registration
        String username, password, phone;
        while (true) {
            System.out.print("Enter username: ");
            username = sc.nextLine();
            if (checkUsername(username)) {
                System.out.println("Username successfully captured");
                break;
            } else {
                System.out.println("Username is not correctly formatted. Must contain '_' and be max 5 chars");
            }
        }
        
        while (true) {
            System.out.print("Enter password: ");
            password = sc.nextLine();
            if (checkPasswordComplexity(password)) {
                System.out.println("Password successfully captured");
                break;
            } else {
                System.out.println("Password must be 8+ chars, have capital, number, and special char");
            }
        }
        
        while (true) {
            System.out.print("Enter cell phone (+27XXXXXXXXX): ");
            phone = sc.nextLine();
            if (checkCellPhoneNumber(phone)) {
                System.out.println("Cell phone successfully added");
                break;
            } else {
                System.out.println("Cell phone incorrectly formatted");
            }
        }
        
        // Login
        System.out.println("\n=== LOGIN ===");
        while (true) {
            System.out.print("Enter username: ");
            String user = sc.nextLine();
            System.out.print("Enter password: ");
            String pass = sc.nextLine();
            
            if (loginUser(user, pass, username, password)) {
                System.out.println("Welcome " + username + ", it is great to see you again.");
                break;
            } else {
                System.out.println("Username or password incorrect, please try again");
            }
        }
        
        // Main menu
        int choice;
        do {
            System.out.println("\n=== MAIN MENU ===");
            System.out.println("1. Send Message");
            System.out.println("2. Show Sent Messages");
            System.out.println("3. Quit");
            System.out.print("Choose: ");
            choice = sc.nextInt();
            sc.nextLine();
            
            switch (choice) {
                case 1:
                    sendMessage(sc);
                    break;
                case 2:
                    printMessages();
                    break;
                case 3:
                    saveMessagesToFile();
                    System.out.println("Messages saved. Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice");
            }
        } while (choice!= 3);
        
        sc.close();
    }
    
    public static boolean checkUsername(String username) {
        return username.contains("_") && username.length() <= 5;
    }
    
    public static boolean checkPasswordComplexity(String password) {
        if (password.length() < 8) return false;
        boolean hasCap =!password.equals(password.toLowerCase());
        boolean hasNum = password.matches(".*\\d.*");
        boolean hasSpecial = password.matches(".*[!@#$%^&*()].*");
        return hasCap && hasNum && hasSpecial;
    }
    
    public static boolean checkCellPhoneNumber(String phone) {
        return phone.matches("\\+27\\d{9}");
    }
    
    public static boolean loginUser(String inputUser, String inputPass, String realUser, String realPass) {
        return inputUser.equals(realUser) && inputPass.equals(realPass);
    }
    
    public static String checkMessageID(String id) {
        return (id.length() == 10)? "Message ID successfully created" : "Message ID is incorrectly formatted";
    }
    
    public static String createMessageHash(String id, int num, String message) {
        String[] words = message.toUpperCase().split(" ");
        String firstWord = words.length > 0? words[0] : "";
        String lastWord = words.length > 1? words[words.length - 1] : firstWord;
        return id.substring(0, 2) + ":" + num + ":" + firstWord + lastWord;
    }
    
    public static void sendMessage(Scanner sc) {
        System.out.print("Enter recipient number (+27XXXXXXXXX): ");
        String recipient = sc.nextLine();
        
        if (!checkCellPhoneNumber(recipient)) {
            System.out.println("Invalid recipient number");
            return;
        }
        
        System.out.print("Enter message: ");
        String content = sc.nextLine();
        
        if (content.length() > 250) {
            System.out.println("Message exceeds 250 characters. Please reduce size");
            return;
        }
        
        String messageId = String.format("%010d", (long)(Math.random() * 10000000L));
        String hash = createMessageHash(messageId, messageCount, content);
        
        System.out.println("\n1. Send Message");
        System.out.println("2. Disregard Message");
        System.out.println("3. Store Message to Send Later");
        System.out.print("Choose: ");
        int option = sc.nextInt();
        sc.nextLine();
        
        if (option == 1) {
            messages.add(new Message(messageId, recipient, content, hash));
            messageCount++;
            System.out.println("Message sent successfully");
            System.out.println("Message ID: " + messageId);
            System.out.println("Message Hash: " + hash);
        } else if (option == 2) {
            System.out.println("Message disregarded");
        } else if (option == 3) {
            messages.add(new Message(messageId, recipient, content, hash));
            messageCount++;
            System.out.println("Message stored");
        }
    }
    
    public static void printMessages() {
        if (messages.isEmpty()) {
            System.out.println("No messages sent");
            return;
        }
        for (Message m : messages) {
            System.out.println("\n---");
            System.out.println("To: " + m.recipient);
            System.out.println("Message: " + m.content);
            System.out.println("ID: " + m.messageId);
            System.out.println("Hash: " + m.hash);
        }
    }
    
    public static void saveMessagesToFile() {
        try (FileWriter fw = new FileWriter("messages.json")) {
            fw.write("[\n");
            for (int i = 0; i < messages.size(); i++) {
                Message m = messages.get(i);
                fw.write(" {\n");
                fw.write(" \"messageId\": \"" + m.messageId + "\",\n");
                fw.write(" \"recipient\": \"" + m.recipient + "\",\n");
                fw.write(" \"content\": \"" + m.content + "\",\n");
                fw.write(" \"hash\": \"" + m.hash + "\"\n");
                fw.write(" }");
                if (i < messages.size() - 1) fw.write(",");
                fw.write("\n");
            }
            fw.write("]");
            System.out.println("Saved to messages.json");
        } catch (IOException e) {
            System.out.println("Error saving file: " + e.getMessage());
        }
    }
}
