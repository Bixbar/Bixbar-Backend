# Bixbar-Backend

## Project Architecture
### Stack
- Django

### Directory structure
- ๐ Bixbar
  - settings.py
  - urls.py
  - wsgi.py
- ๐ Service
  - ๐ CocktailRecommendation
    - CreateEventTracker.py
    - GetRecommendation.py
    - PutEvent.py
  - models.py
  - veiws.py
- manage.py

## Code review
### HttpResponseRedirect()
```python
return HttpResponseRedirect('https://www.notion.so/ccookncook/Bixbar-b5401104a0d64fdc838d27505fbf27b2')
```
- url์ด www.Bixbar.com ์ด๋ฉด Notion Page๋ก Redirectํด์ค๋ค.

### Papago API
```python
try: # ๊ธ์ ํ๋ ์ด๊ณผ์ ์ค๋ฅ ๋ฐฉ์ง
  url = 'https://openapi.naver.com/v1/papago/n2mt' # Papago API url
  headers = { 
      'X-Naver-Client-Id': 'Client-Id',
      'X-Naver-Client-Secret': 'Client-Secret', 
  }

  # recipe ๋ฒ์ญ
  i = 0
  for reList in recipeList:
      params = { 
          'source': 'en', 
          'target': 'ko', 
          'honorific': 'true', # ๋์๋ง
          'text': reList, # ๋ฒ์ญํ  Text
      }

      res = requests.post(url, headers=headers, data=params) # post ํ์์ผ๋ก request
      result = res.json() # json ํ์์ผ๋ก ๋ฐ์์ด
      translatedRecipe = result['message']['result']['translatedText'] # ๋ฒ์ญ๋ Text๋ง ์ถ์ถ
      recipeList[i] = translatedRecipe
      i+=1

except: # ์๋ฌธ์ผ๋ก ๋ณต๊ตฌ
            recipeList = bakRecipeList
```
- Papago API๋ฅผ ์ฌ์ฉํ์ฌ ๋ ์ํผ๋ฅผ ๋ฒ์ญํ๋ค.
  -  `'honorific': 'ture',` : ์์ฐ์ค๋ฌ์ด ๋ ์ํผ ์ ๊ณต์ ์ํ์ฌ ๋์๋ง๋ก ๋ฒ์ญํ๋ค.

### HttpResponse()
```python
# GET request์ ์ธ์ ์ค q ๊ฐ์ด ์์ผ๋ฉด ๊ฐ์ ธ์ด, ์์ผ๋ฉด ๋น ๋ฌธ์์ด
q = request.GET.get('q','')
```
- `get` http://www.bixbar.com/cocktail/?q=screw
  - q์ ๊ฐ์ ์ด์ฉํ์ฌ ์กฐํ๋ฅผ ์ํํ๋ค.

```python
group_items.append( {'title': title, 'img': item[2], 'recipe': recipeList, 'glass': item[4], 'garnish': item[5], 'liquor': liquorList, 'liquorml': mlList, 'flavor': flavorList, 'baseSpirit': baseList, 'cocktailType': typeList, 'served': servedList, 'preparation': prepList, 'strength': strengthList, 'difficulty': diffList, 'hours': hoursList, 'brands': brandList} )
jsonqs = json.dumps(group_items)
return HttpResponse(jsonqs, content_type="text/json")
```
- json.dumps()
  -  objectํ์์ JSONํ์ ์คํธ๋ฆผ์ผ๋ก ์ง๋ ฌํํ๋ค.
-  HttpResponse()
    -  JSON์ผ๋ก ๋ฐ์ดํฐ๋ฅผ Responseํ๋ค.

