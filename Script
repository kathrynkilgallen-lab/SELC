# The exact URL you provided for North Carolina filtered press releases
url = "https://www.selc.org/press-release/?_states=north-carolina"

# Define a standard browser header so the request flows smoothly
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"
}

print(f"Fetching data from: {url} ...")
response = requests.get(url, headers=headers)

if response.status_code == 200:
    soup = BeautifulSoup(response.text, 'html.parser')

    # We loop through the identified press release containers to extract metadata
    press_releases = []

    # We find all <a> tags with the class 'small-grid' which encapsulate each press release
    articles = soup.find_all('a', class_='small-grid')

    for article in articles:
        # 1. Grab the headline text from the h2 tag within the article
        title_element = article.find('h2', class_='u-h4')
        title = title_element.get_text(strip=True) if title_element else "Untitled"

        # 2. Extract the link from the 'href' attribute of the <a> tag itself
        link = article['href'] if article.has_attr('href') else ""
        if link and not link.startswith('http'):
            link = f"https://www.selc.org{link}"

        # 3. Pull the date text from the span with class 'u-eyebrow'
        date_element = article.find('span', class_='u-eyebrow')
        date = date_element.get_text(strip=True) if date_element else "Date not found"

        # Capture the whole string snippet for keyword filtering
        full_card_text = article.get_text().lower()

        press_releases.append({
            "Date": date,
            "Title": title,
            "Link": link,
            "Full_Text": full_card_text
        })

    # Convert list to an interactive Pandas DataFrame
    df = pd.DataFrame(press_releases)
    print(f"Successfully loaded {len(df)} recent North Carolina press releases!")
else:
    print(f"Failed to fetch page. Status code: {response.status_code}")
df.to_csv('SELC.CSV', index=False)
