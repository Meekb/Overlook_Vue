# Overlook Hotel - Currently Under Construction 🦺🛠

## Overview
This is a rebuild of an earlier vanilla JS project called Overlook. It's a hotel booking application which allows a user to login with a username and password, view the total amount of money spent at the hotel, view details of their previous stays, and book future stays. The project is under construction using Vue 3 with Nuxt.js so **PLEASE PARDON THE MESS**. End to end testing will be completed using Cypress.

*[The original spec for Overlook can be found here](https://frontend.turing.edu/projects/overlook.html)*  
*[Instructions for cloning and running the Overlook-api can be found here](https://github.com/turingschool-examples/overlook-api)*

## Instructions

```bash
# clone down this repo
$ git clone git@github.com:Meekb/Overlook_Vue.git 
$ cd into project

# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev
```
## Project Details
  * Built with Vue 3 using components
  * Page routing will be handled by Nuxt.js - **UNDER CONSTRUCTION**
  * Username: 'customer01' to 'customer50'
  * Password: 'overlook2021'
  * API holds data for users, rooms, and bookings
    * Bookings are for the year 2020  
    * No error handling for past check-in dates which currently allows for POSTs in 2020
  * GET and POST implemented with Axios
  * Application testing will be completed using Cypress - **UNDER CONSTRUCTION**

## Walkthrough of Overlook at current stage of development

Providing an incorrect username or password will throw an error for the user then clear the input fields for re-entry
![overlook-login](https://user-images.githubusercontent.com/76264735/132587112-3c6df0dc-fcee-4acb-a649-726d4306d11f.gif)

Once successfully logged in, the user can view their history with Overlook Hotel including the total dollars they've spent. 
Wow, that's a lot of money! This hotel must be REALLY popular
![overlook-history](https://user-images.githubusercontent.com/76264735/132587356-a83bc8e0-87a1-4177-ad3e-de072501783b.gif)

User searches by providing a check-in date and selecting a room type
![overlook-search](https://user-images.githubusercontent.com/76264735/132585053-2a229971-be18-4393-9bb7-445673d8bd07.gif)

If no rooms of that type are available for the check-in date, the user will receive a strongly worded message advising them to 
change the room type or adjust their check-in date
![overlook-ahshit](https://user-images.githubusercontent.com/76264735/132585448-e57f4f45-83bd-4861-bdb8-700ee5be98f7.gif)

Once a room has been booked, the user will receive a success message and confirmation number for their records
![overlook-confirmation](https://user-images.githubusercontent.com/76264735/132585522-afd0469d-284a-4659-8102-878e8e721b8d.gif)

## Tech Stack
<table>
  <tr>
    <td>Vue 3</td>
    <td>Nuxt.js</td>
    <td>JavaScript ES6</td>
    <td>CSS</td>
    <td>Cypress</td>
  </tr>
  <tr>
    <td><img width="55" src="https://raw.githubusercontent.com/gilbarbara/logos/master/logos/vue.svg"/></td>
    <td><img width="55" src="https://raw.githubusercontent.com/gilbarbara/logos/master/logos/nuxt.svg"/></td>   
    <td><img width="55" src="https://raw.githubusercontent.com/gilbarbara/logos/master/logos/javascript.svg"/></td>
    <td><img width="55" src="https://raw.githubusercontent.com/gilbarbara/logos/master/logos/css-3.svg"/></td>
    <td><img width="55" src="https://raw.githubusercontent.com/gilbarbara/logos/master/logos/cypress.svg"/></td>
  </tr>
</table>

## Contributors
<table>
  <tr>
   <td> Beth Meeker <a href="https://github.com/meekb">GH</td>
  </tr>
  </tr>
    <td><img src="https://avatars.githubusercontent.com/u/76264735?v=4" alt="Beth Meeker avatar"
    width="150" height="auto" /></td>
  </tr>
</table>

[Turing School of Software & Design - Original Overlook spec](https://frontend.turing.edu/projects/overlook.html)
