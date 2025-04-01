# Nice to Meet You!
This is a page about the fun things I do in my free time!
Mainly related to PCs and applications/softwares because I love them!

## About me
Name: Chris <br>
Job: Developer <br>
Hobby: Gaming, Animes, DIY PC

## Intro
I am an IT engineer living in Japan. <br>
I likes to do some small projects in my free time and want to share my hobby with others. <br>
That's why I created this github pages.<br>
I hope that I can add more fun stuffs hear as I work along!

# Posts
{% for tag in site.tags %}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}