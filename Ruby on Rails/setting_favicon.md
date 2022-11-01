## faviconを設定する

public直下にfavicon.icoを置く

app/views/layouts/application.html.erbのheadタグ内で

`<%= favicon_link_tag '/favicon.ico' %>`
