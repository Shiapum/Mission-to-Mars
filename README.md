# Mission-to-Mars

## Overview of Project

### Purpose
Use BeautifulSoup and Splinter to scrape full-resolution images of Mars’s hemispheres and the titles of those images, store the scraped data on a Mongo database, use a web application to display the data, and alter the design of the web app to accommodate these images.

### Background
Robin's web app is looking good and functioning well, but she wants to add more polish to it. She had been admiring images of Mars’s hemispheres online and realized that the site is scraping-friendly. She would like to adjust the current web app to include all four of the hemisphere images.

## Mars Hemispheres
![image](https://user-images.githubusercontent.com/21972342/143831247-fc0ea01a-a0f1-4d33-9838-02435b09477e.png)

Images were obtained using following code:
```
def mars_hemispheres(browser):

    url = 'https://marshemispheres.com/'
    browser.visit(url)

    # 2. Create a list to hold the images and titles.
    hemispheres_images_urls = []
    hemispheres = []
    relatives = []
    title = []
    
    # 3. Write code to retrieve the image urls and titles for each hemisphere.
    html = browser.html
    hem_soup = soup(html, 'html.parser')
    results = hem_soup.find_all('a', class_='itemLink product-item')
    
    for result in results:
        relative_url = result.get('href')
        if relative_url not in relatives:
            try:
                relatives.append(relative_url)
                browser.visit('https://marshemispheres.com/'+ relative_url)
                title.append(browser.find_by_tag('h2').text)
                html = browser.html
                img_soup = soup(html, 'html.parser')
                img_url_rel = img_soup.find_all('a')[3].get('href')
                hemispheres.append('https://marshemispheres.com/'+img_url_rel)
            except BaseException:
                return None

    for x in range(4):
        hemispheres_images_urls.append({'image_url':hemispheres[x], 'title':title[x]})

    return hemispheres_images_urls

```


