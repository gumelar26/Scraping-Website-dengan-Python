#### Scraping data Times_Fiction_Best_Sellers_2015 (with Python)

    import bs4
    from urllib.request import urlopen as uReq
    from bs4 import BeautifulSoup as soup

    my_url = 'https://www.goodreads.com/list/show/83612.NY_Times_Fiction_Best_Sellers_2015'
    my_url

    uClient = uReq(my_url)
    page_html = uClient.read()
    uClient.close()

    page_soup = soup(page_html, "html.parser")

    tds = page_soup.findAll("td", {"width":"100%","valign":"top"})

    file = "goog_reads.csv"
    header = "title,author,rating,score \n"
    f = open(file, "w")
    f.write(header)


    for td in tds :
        books = td.a.select("span")
        title = books[0].text
        authors = td.div.select("a")
        author = authors[0].text
        ratings = td.findAll("span",{"class":"greyText smallText uitext"})
        rating = ratings[0].text.strip().replace(",",".")
        scores = td.findAll("div",{"style":"margin-top: 5px"})
        score = scores[0].span.a.text.strip().replace("score:","").replace(",",".").replace(" ","")
    
    
        print("book_name: "+ title)
        print("author: "+ author)
        print("rating: "+ rating)
        print("goodreads_score: "+ score)
    
        f.write(title.replace(",","") + ", " + author + ", " + rating + ", " + score +"\n")

    f.close()
