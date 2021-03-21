# Multi Master in maria db

Can we configure Multi Master in Maria Db. I have testing three node A, B, C in Maria DB in Galera cluster. This 3 node belongs to one site. I want to configure HA with another site having the same setting as A1,B1, C1. Now i want to make A and B as master of C1. A,B,C, A1,B1,and C1 will have same db. Will it maintain the txId while replication?