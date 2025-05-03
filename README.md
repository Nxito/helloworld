# Repo para EU - DevOps&Cloud - UNIR

Este repositorio incluye un proyecto sencillo para demostrar los conceptos de pruebas unitarias, pruebas de servicio, uso de Wiremock y pruebas de rendimiento
El objetivo es que el alumno entienda estos conceptos, por lo que el código y la estructura del proyecto son especialmente sencillos.
Este proyecto sirve también como fuente de código para el pipeline de Jenkins.

variables necesarias .env

- JENKINS_AGENT_SSH_PUBKEY : Clave para los agentes ssh de jenkins se utilizará ssh-keygen para generar la clave. Se indica aqui el contenido del archivo .pub generado. En jenkins se suará la clave generada
