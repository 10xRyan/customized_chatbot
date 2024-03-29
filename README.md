# customized chatbot

- Implementation 1: idle to be disconnected
   
While I reflected on possible ways that allow the bot to be disconnected without entering some specific instructions such as “quit” and “exit” defined in the code, or “ctrl+D,” I found this appears to be impossible. This is because the only way to make this happen is to let the bot understand what we are talking about. However, being a progressive text generator, this chatbot, or any published LLM, could not understand the meaning of the sentences we delivered.

Therefore, I generated an alternative way to exit the chat by setting a special timer: if users do not reply within a certain time, the chat will be automatically disconnected.

- Implementation 2: Let the bot to draft and send an email

I utilized the 'smtplib' library for sending emails. For the content, I used a quite naive way to identify a certain pattern of the drafted email created by the chatbot and parsed it by subject and body. Moreover, I asked the user to provide the recipient email and use the 'smtplib' to send the email.

- Implementation 3: Access to Chat History

I aim to design a feature, similar to that of ChatGPT, which enables users to access previous conversations. To do so, I created a local folder to record the chat history and place each conversation in corresponding .txt files. When users proceed to continue with the conversation, the box finds its corresponding .txt file and summarizes it. Then, users could continue with their discussions.


##  What TODO Next

- First, concerning implementation #1, we can build a sentiment analysis model to let the bot “understand” a will to leave the conversation. Creating a learning dataset that includes various expressions of the will, such as “I want to end the conversation” and “goodbye,” we can feed the dataset to an architecture and train the sentiment analysis model to predict whether the chatbot should end the chat. 

(I will look for an alternative way to implement a timer feature, and I will explain the motivation in the next section). 

- In addition, if time allows, I will build a classifier to check whether the email addresses provided by users are valid. 

Furthermore, another issue is that naively extracting the drafted emails created by the chatbot makes the system collapse if the bot does not provide the emails in their ideal format. One solution involves letting the bot reply in a specific format. Nevertheless, it’s difficult to let the bot know when to send the email format answer. Therefore, I think we should employ this method only if we intend to build a bot primarily responsible for sending emails, (i.e. while users elaborate on the content of the email they intend to send in 3 cycles of conversations, the bot replies with the email format in the fourth cycle).

- Finally, while currently, I write explicitly in my code “help me to write a review of this dialogue (‘previous dialogue’)” to let the bot provide chat history, I aim to avoid hard coding in the next step. 

## Analysis

- There is a problem with my current design of the timer feature: since I used a multi-threading and non-blocking system design, there are limited features that we could add to the model. 

- Moreover, the sending email feature is running smoothly because the emails generated by the chatbot are usually in their ideal format. Since we use the word "Subject:'' to identify if the chatbot is giving us a drafted email, the metric I will use is the percentage of word "Subject:" appears in a non-email response from the chatbot. It will be the error rate for mistakenly identifying sending an email. Similarly, this will apply to the words “Dear” and "[Your name]" I used for parsing the subject and body of the email.

- Since I hard coded the bot to analyze the text in the previous dialogue file, there will be a problem if we revisit the dialogue more than one time. For example, at the first time when we request to revisit the dialogue, the script would be "help me to summarize(dialogue)". But the script would become "help me to summarize(help me to summarize(dialogue))" in our second try. Although the chatbot might be able smart enough to overcome this problem during the first several revisits, in deep revisits, as the majority of the space in the previous chat would have been occupied by "help me to summarize...", the chatbot will then face a difficulty in analyzing the request.

Similarly, if we try to revisit a previous chat history contains a potential drafted email, the chatbot will have a difficulty to summarize that.
