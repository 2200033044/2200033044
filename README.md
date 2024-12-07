package com.klef.jfsd.exam;

import org.hibernate.*;
import org.hibernate.cfg.Configuration;

import java.util.List;

public class ClientDemo {
    public static void main(String[] args) {
        // Create session factory
        SessionFactory factory = new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Client.class).buildSessionFactory();

        try (factory) {
            Session session = factory.openSession();
            Transaction tx = session.beginTransaction();

            // I. Insert Records
            Client client = new Client();
            client.setName("John Doe");
            client.setGender("Male");
            client.setAge(30);
            client.setLocation("New York");
            client.setEmail("john.doe@example.com");
            client.setMobile("1234567890");
            session.save(client);

            // II. Retrieve and Print All Records
            List<Client> clients = session.createQuery("from Client", Client.class).list();
            for (Client c : clients) {
                System.out.println("ID: " + c.getId() + ", Name: " + c.getName() + ", Gender: " + c.getGender() +
                        ", Age: " + c.getAge() + ", Location: " + c.getLocation() +
                        ", Email: " + c.getEmail() + ", Mobile: " + c.getMobile());
            }

            tx.commit();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
