- 👋 Hi, I’m @WizooBlackChat
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
WizooBlackChat/WizooBlackChat is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

   
{% extends "base.html" %}
{% load humanize %}
{% load markdown %}
{% load chat_tags %}

{% block content %}
<style>
    .special-height {
        max-height: calc(100vh - 58px);
        min-height: calc(90vh - 58px);

    }

    .chat-area {
        display: flex;
        background: #fff;
        flex-direction: column-reverse;
        height: 100%;
        min-height: 80vh;
        max-height: 89vh;
        overflow: auto;
    }

    .chat-area img.img-thumbnail.rounded-circle {
        width: 50px !important;
        height: 50px !important;
    }

    .message-input-area {
        display: flex;
        padding: 10px 20px 5px;
        background: rgba(0, 0, 0, 0.09);
        flex-direction: row-reverse;
        
        
        width: 100%;
        padding-top: 10px;

    }
    .message-input-area > span{
        height: 100%;
        display: flex;
        align-items:center;
        justify-content: center;
        box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
        background: #555;

    }
    blockquote {
        border-left: 3px solid rgb(192, 192, 192, .8);
        font-size: 14px;
        margin: 5px 0;
        padding: 5px 10px;
        background: rgba(248, 248, 248, 0.5);
    }

    .message-input-area #message-input,
    .message-input-area button {
        margin: 0;
        width: 100%;
        border-radius: 25px 0px 0px 25px;
        border: none;
        padding: 5px 20px;
        background: #555;
        color: #ddd;
        outline: none;
        box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);

    }

    .message-input-area button {
        width: unset;
        border-radius: 0 25px 25px 0;
        background: #333;
        color: #999;
        padding-top: 0;
        padding-bottom: 0;
    }

    .chat-user-img {
        width: auto;
    }

    .chat-panel {
        height: 100%;
        overflow-y: scroll;

    }

    .user-message-container {
        display: flex;
        flex-direction: row;
        gap: 10px;
        padding: 5px;
    }

    .chat-area img {
        width: 45px;
        height: 45px;
    }

    .user-message {
        display: flex;
        flex-direction: column;
        justify-content: right;
        width: 100%;
    }

    .sent {
        padding-left: 10%;
        justify-content: right;
        flex-direction: row-reverse;
        
    }

    .received {
        padding-right: 10%;
    }

    .received .user-message {
        text-align: left;
    }

    .sent span {
        background: rgba(0, 0, 0, 0.05);
    }

    .received span {
        background: rgb(255, 165, 31);
        color: black;
    }
    .sent .user-message{
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
        align-items: flex-end;
    }
    .user-message-content span {
        display: inline-block;
        box-shadow: 0 0 3px rgba(0, 0, 0, 0.09);
        padding: 5px 10px;
        width: auto;
        border-radius: 7px 10px;
    }
    .user-message-content *{
        overflow-wrap: break-word;
    }
    .search-contact {
        background-color: black;
        padding: 9px;
    }

    @media only screen and (max-width: 650px) {
        .small-screen {
            display: none;
        }
    }

    @media only screen and (min-width: 650px) {
        .big-screen {
            display: none;
        }
    }
    .markdown {
        display: inline-block;
        padding: 0;
        margin: 0;
        line-height: unset;
    }
    .markdown * {
        margin: 0;
    }
    .markdown a{
        color:rgb(215, 165, 31);
    }
    .main {
        display: flex;
        flex-direction: column;
        height: 100vh;
    }
</style>

<div class="container-fluid forum-lg-100-overflow-hidden">

    <div class="row forum-md-100-fixed">

        <!-- contact area -->
        <div class="col-lg-4 col-md-4 white-bg forum-md-100-fixed">
            <div class="big-screen">
                <button class="btn btn-dark shadow-none btn-block" type="button" data-toggle="collapse"
                    data-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
                    <i class="fas fa-users mr-3"></i> Contacts
                </button>
                </p>
                <div class="collapse" id="collapseExample">
                    <div class="search-contact mb-2">
                        <input type="text" class="user-search" placeholder="search">
                    </div>
                    {% for user in users %}
                    <div class="contacts" data-username="{{ user.username }}">

                        <div class="pb-1 d-flex flex-row  align-items-center">
                            <div class="h-100">
                                
                                    <img style="width: 60px; height: 60px;" class="img-thumbnail rounded-circle"
                                        src="{{ user.profile.picture.url }}">
                              
                            </div>
                            <div class="card-body py-1">
                                <a href="{% url 'chatroom' user.pk %}" class="d-flex d-row-flex justify-content-between">
                                    <div class="info">
                                        <p class="font-large font-weight-bold color-primary f p-0 m-0">
                                            {{ user.username }}
                                        </p>
                                        
                                    </div>
                                    {% if user|getunreadmessages:request.user > 0 %}
                                    <div class="new_msg float-right">
                                        <i class="badge badge-warning">{{ user|getunreadmessages:request.user }}</i>
                                    </div>
                                    {% endif %}
                                </a>
                            </div>
                        </div>
                    </div>
                    {% empty %}
                    
                    <p>There are no Users</p>
               
                    {% endfor %}
                    <p class="d-none chat-empty-tag">No item matches your query</p>
                </div>
            </div>

            <div class=" shadow-sm small-screen forum-md-100-fixed" style="max-height: calc(99vh - 66px); overflow-y: auto;">
                <div class="search-contact mb-2">
                    <h3 class="text-white">All Users</h3>
                </div>
                {% for user in users %}
                <div class="contacts" data-username="{{ user.username }}">

                    <div class="pb-1 d-flex flex-row align-items-center ">
                        <div class="h-100">
                            
                                <img style="width: 60px; height: 60px;" class="img-thumbnail rounded-circle"
                                    src="{{ user.profile.picture.url }}">
                            
                        </div>
                        <div class="card-body py-1">
                            <a href="{% url 'chatroom' user.pk %}" class="d-flex d-row-flex justify-content-between">
                                <div class="info">
                                    <p class="font-large font-weight-bold color-primary f p-0 m-0">
                                        {{ user.username }}
                                    </p>
                                    
                                </div>
                                
                            </a>
                        </div>
                    </div>
                </div>
                {% empty %}
               
                <p>There are no users </p>
                
                {% endfor %}
                <p class="d-none chat-empty-tag">No item matches your query</p>

            </div>
        </div>


        <!-- chat area -->
        <div class="col-lg-8 col-md-8">
            <div class=" chat-area">
                <div class="message-input-area p-0 p-sm-2">
                    <button id="message-send-btn" style="font-size: 40px;"><i class="fas fa-caret-right"></i></button>
                    
                    <textarea class="py-2" rows="2" placeholder="type your message" id="message-input" style="height: unset;"></textarea>
                </div>
                <div class="markdown d-none justify-content-between" id="reply-message-container">
                    <blockquote class="w-100">
                    <p><em id="reply-message"></em></p>
                    </blockquote>
                    <button id="reply-message-cancel" class="m-0 bg-white p-2 text-danger" style="border: none; outline: none;">&times;</button>
                </div>
                <div id="chat-panel" class="chat-panel">

                    {% for user_message in user_messages %}
                    <div
                        class="user-message-container {% if request.user == user_message.sender %} sent {% else %} received{% endif %}">
                        <div class="chat-user-img">
                            <img class="img-thumbnail  rounded-circle"
                                src="{{ user_message.sender.profile.picture.url }}">
                        </div>

                        <div class="user-message text-wrap">
                            <div class="user-message-content text-wrap text-break">
                                <span class="inline-block text-wrap">
                                    <div class="d-flex flex-row justify-content-between">
                                        <div id="user-message-{{ user_message.pk }}">
                                            {{ user_message.message | markdown }}
                                        </div>

                                        <div class="dropdown m-0 p-0">
                                            <span class="category-name p-0 m-0" type="button" id="dropdownMenuButton"
                                                data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                                                    <i class="fas fa-caret-down fa-lg ml-2"></i>
                                            </span>
                    
                                            <div class="dropdown-menu dropdown-warning  dropdown-menu-right shadow-sm"
                                                aria-labelledby="dropdownMenuButton">

                                                <a class="reply-btn dropdown-item p-1" data-username="{{ user_message.sender.username }}" data-message="{{ user_message.get_reply_message }}" >Reply</a>
                                                {% if user_message.sender == request.user %}
                                                <a class="dropdown-item p-1 delete-message-btn"  data-url="{{ user_message.get_delete_url }}" data-target="user-message-{{ user_message.pk }}">
                                                    Delete
                                                </a>
                                                {% endif %}
                            
                                            </div>
                                            
                                        </div>

                                    </div>
                                </span>
                            </div>
                            <div class="user-message-date font-xsmall">
                                {{ user_message.date_created | naturaltime}}
                            </div>
                        </div>

                    </div>

                    {% endfor %}
                </div>

                <div class="bg-dark d-flex align-items-center px-3 px-md-5 py-1">
                    <img class="img-thumbnail rounded-circle" style="height: 60px !important; width: 60px !important;"
                        src="{{ other_user.profile.picture.url }}">
                    <p class="text-weight-bold font-xlarge pl-3 m-0 text-white">{{ other_user.username }} <br>
                    </p>
                <a class="btn btn-danger ml-auto" href="{% url 'logout' %}">Log Out</a>

                </div>

            </div>
        </div>
    </div>
</div>










<script defer async>
    let message_send_btn = document.getElementById("message-send-btn");
    let reply_message = "";
    let message_input = document.getElementById("message-input");
    let interval = 0;
    let load_counter = 0;
    let current_interval = 2000;
    let reply_message_container = document.getElementById("reply-message-container");
    let reply_message_element = document.getElementById("reply-message");
    let reply_message_cancel = document.getElementById("reply-message-cancel");

    function cancel_reply() {
        reply_message = "";
        reply_message_element.innerHTML = "";
        reply_message_container.classList.add("d-none");
        reply_message_container.classList.remove("d-flex");
    }
    reply_message_cancel.addEventListener('click',cancel_reply );
    function reply_btn_event_handler(e) {

        reply_message = "> [" + this.dataset.username + "]()\n\n" + "> " + this.dataset.message + "\n\n";
        reply_message_element.innerHTML = '<a class="text-warning">' + this.dataset.username +'</a><br>' + this.dataset.message;
        reply_message_container.classList.remove("d-none");
        reply_message_container.classList.add("d-flex");
    }
    function construct_reply_events (){
        let reply_btns = document.getElementsByClassName("reply-btn");
        for (reply_btn of reply_btns) {
            reply_btn.removeEventListener("click", reply_btn_event_handler);
            reply_btn.addEventListener("click", reply_btn_event_handler);
        }
    }
    construct_reply_events();
    function send_message() {
        message = message_input.value;
        if (message === "") {
            return;
        }
        message.replaceAll('\n', '\n\n');
        message_input.value = "";
        fetch("{% url 'chatroom-ajax' other_user.pk %}", {
            method: 'POST',
            credentials: 'same-origin',
            headers: {
                'Content-Type': 'application/json',
                'X-CSRFToken': "{{ csrf_token }}",
            },
            body: JSON.stringify({ "message": reply_message + message })
        }).then(e => e.json()).then(messages => {
            reply_message = "";
            for (message of messages) {
                construct_message(message);
            }
            cancel_reply();
            construct_reply_events()
        }).catch(e => {console.log(e); alert("There was an error sending your message.")});
    }

    function load_messages() {
        fetch("{% url 'chatroom-ajax' other_user.pk %}").then(e => e.json()).then(messages => {
            // these if else statements are there to avoid flooding the server with lots of requests per second.
            if (messages.length > 0) {
                if (current_interval > 2000) {
                    current_interval = 2000;
                    load_counter = 0;
                    clearInterval(interval);
                    interval = setInterval(load_messages, current_interval);
                }

            } else {
                load_counter++;
                if (load_counter > 2) {
                    if (current_interval < 5000) {
                        current_interval = current_interval + 1000;
                    }
                    clearInterval(interval)
                    interval = setInterval(load_messages, current_interval);
                }
            }
            for (message of messages) {
                construct_message(message);
            }
            construct_reply_events();
        }).catch(e => console.log(e));
    }
    function delete_message(e){
        let url = this.dataset.url;
        let target = document.getElementById(this.dataset.target);
        fetch(url).then(e => e.json()).then(data => {
            if (data.success){
                target.innerHTML = data["content"];
            }
        }).catch(e => console.log(e));
    }
    function delete_messages_constructor(){
        for (delete_btn of document.getElementsByClassName("delete-message-btn") ){
            delete_btn.removeEventListener("click", delete_message);
            delete_btn.addEventListener("click", delete_message)
        }

    }
    delete_messages_constructor()
    function construct_message(message) {
        let user_message_container = document.createElement("div")
        if (message["sent"]) {
            user_message_container.classList.add("user-message-container", "sent");

        } else {
            user_message_container.classList.add("user-message-container", "received");
        }
        // construct image section
        let chat_user_img = document.createElement("div");
        chat_user_img.classList.add("chat-user-img");
        let image = document.createElement("img");
        image.src = message["picture"];
        image.classList.add("img-thumbnail", "rounded-circle");
        chat_user_img.appendChild(image);
        // construct message content
        //<div class="user_message ">
        //    <div class="user_message-content"><span>{{ user_message }}</span></div>
        //    <div class="user_message-date">{{ user_message.date_created | naturaltime }}</div>
        // </div>
        let user_message = document.createElement("div");
        user_message.classList.add("user-message");

        let user_message_content = document.createElement("div");
        user_message_content.classList.add("user-message-content");
        let span = document.createElement("span");
        span.classList.add("inline-block", "text-wrap")
        user_message_content.appendChild(span);
        let delete_btn = '';
        if (message["sent"]){
            delete_btn = `
            <a class="dropdown-item p-1"  data-url="${message['get_delete_url']}" data-target="user-message-${message['pk']}">
                Delete
            </a>`;
        }
        span.innerHTML += `
        <div class="d-flex flex-row justify-content-between">
            <div id="user-message-${message['pk']}">
                ${message["message"]}
            </div>
            
        </div>
        `

        let user_message_date = document.createElement("div");
        user_message_date.classList.add("user-message-date", "font-xsmall");
        user_message_date.innerText = message["date_created"];
        user_message.appendChild(user_message_content)
        user_message_content.appendChild(user_message_date)

        user_message_container.appendChild(chat_user_img);
        user_message_container.appendChild(user_message);

        let chat_panel = document.getElementById("chat-panel");
        chat_panel.appendChild(user_message_container);
        user_message_container.scrollIntoView();
        delete_messages_constructor()

    }
    function scrolllastmessageintoView() {
        let messages = document.getElementsByClassName("user-message-container");
        if (messages.length > 0) {
            messages[messages.length - 1].scrollIntoView();
        }
    }

    message_send_btn.addEventListener("click", send_message)
    message_input.addEventListener("keydown", (e) => {
        /* if (e.key === 'Enter') {
            send_message()
        } */
    });
    scrolllastmessageintoView()
    interval = setInterval(load_messages, current_interval);
</script>


<script>
    window.onload = function () {
        let contacts = document.getElementsByClassName("contacts");
        let search_inputs = document.getElementsByClassName("user-search");
        search_inputs.forEach(search_input => {
            search_input.addEventListener("input", function () {
                let val = search_input.value;
                let empty = true;
                contacts.forEach(element => {
                    if (element.dataset.username.toLowerCase().search(val.toLowerCase()) >= 0) {
                        empty = false;
                        element.classList.remove("d-none");
                    } else {
                        element.classList.add("d-none");
                    }

                });
                if (empty) {
                    document.getElementsByClassName("chat-empty-tag").forEach(element => { element.classList.remove("d-none"); });
                } else {
                    document.getElementsByClassName("chat-empty-tag").forEach(element => { element.classList.add("d-none"); });
                }
                empty = false;
            });
        });
    }
</script>
{% endblock %}
