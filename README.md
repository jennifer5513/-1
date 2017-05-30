import requests

from bs4 import BeautifulSoup

import re

HTML_PARSER = "html.parser"

ROOT_URL = 'http://www.baseball-reference.com'

LIST_URL = 'http://www.baseball-reference.com/boxes/'

SPACE_RE = re.compile(r'\s+')


TEAM_PATH=''

resp = requests.get('http://www.baseball-reference.com/teams/CLE/2017.shtml')

resp.text


Soup=BeautifulSoup(resp.text,"html.parser")

soup


def get_link_list():
    resp = requests.get('http://www.baseball-reference.com/boxes/')
    Soup=BeautifulSoup(resp.text,"html.parser")
    if resp.status_code == requests.codes.ok:
        soup = BeautifulSoup(resp.content, HTML_PARSER)
        teams_links_a_tags = soup.find_all('tr','th','a')

        teams_links = []
        for link in teams_links_a_tags:
            teams_links = ROOT_URL + link['href']
            print(teams_links)
            teams_links.append(teams_links)
            parse_teams_information(teams_links)
            
            
def parse_teams_information(teams_links):
    #team_id = re.sub(re.compile(r'^.*/' + SHOP_PATH), '', shop_link).split('-')[0]
    print(team_id)

    req = requests.get(teams_links)
    if req.status_code == requests.codes.ok:
        soup = BeautifulSoup(req.content, HTML_PARSER)
        team_header_tag = soup.find('div', id='standings-upto-AL-E')
        score_tag = team_header_tag.find('span', attrs={'itemprop': 'name'})
        print(re.sub(SPACE_RE, '', score_tag.text))
        category_tag = shop_header_tag.find("p", class_={'cate i'})
        print(re.sub(SPACE_RE, '', category_tag.a.text))
        address_tag = shop_header_tag.find('a', attrs={'data-label': '上方地址'})
        print(re.sub(SPACE_RE, '', address_tag.text))

        gps_str = address_tag['href']
        # print(gps_str)
        gps_str = re.search('/c=(\d+.\d*),(\d+.\d*)/', gps_str).group().replace('/', '')
        # print(gps_str)
        lat = gps_str.split(',')[0]
        lng = gps_str.split(',')[1]
        print(lat.split('=')[1], lng)
