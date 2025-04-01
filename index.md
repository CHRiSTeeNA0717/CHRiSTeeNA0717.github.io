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
{% assign posts_by_month = site.posts | group_by_exp:"post","post.date | date: '%B %Y'" %}
{% for month in posts_by_month %}
  <h3>{{ month.name }}</h3>
  <ul>
    {% for post in month.items %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}