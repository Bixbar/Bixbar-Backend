# Bixbar-Backend

## Project Architecture
### Stack
- Django

### Directory structure
- 📁 Bixbar
  - settings.py
  - urls.py
  - wsgi.py
- 📁 Service
  - 📁 CocktailRecommendation
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
- url이 www.Bixbar.com 이면 Notion Page로 Redirect해준다.

### Papago API
```python
try: # 글자 한도 초과시 오류 방지
  url = 'https://openapi.naver.com/v1/papago/n2mt' # Papago API url
  headers = { 
      'X-Naver-Client-Id': 'Client-Id',
      'X-Naver-Client-Secret': 'Client-Secret', 
  }

  # recipe 번역
  i = 0
  for reList in recipeList:
      params = { 
          'source': 'en', 
          'target': 'ko', 
          'honorific': 'true', # 높임말
          'text': reList, # 번역할 Text
      }

      res = requests.post(url, headers=headers, data=params) # post 형식으로 request
      result = res.json() # json 형식으로 받아옴
      translatedRecipe = result['message']['result']['translatedText'] # 번역된 Text만 추출
      recipeList[i] = translatedRecipe
      i+=1

except: # 원문으로 복구
            recipeList = bakRecipeList
```
- Papago API를 사용하여 레시피를 번역한다.
  -  `'honorific': 'ture',` : 자연스러운 레시피 제공을 위하여 높임말로 번역한다.

### HttpResponse()
```python
# GET request의 인자 중 q 값이 있으면 가져옴, 없으면 빈 문자열
q = request.GET.get('q','')
```
- `get` http://www.bixbar.com/cocktail/?q=screw
  - q의 값을 이용하여 조회를 수행한다.

```python
group_items.append( {'title': title, 'img': item[2], 'recipe': recipeList, 'glass': item[4], 'garnish': item[5], 'liquor': liquorList, 'liquorml': mlList, 'flavor': flavorList, 'baseSpirit': baseList, 'cocktailType': typeList, 'served': servedList, 'preparation': prepList, 'strength': strengthList, 'difficulty': diffList, 'hours': hoursList, 'brands': brandList} )
jsonqs = json.dumps(group_items)
return HttpResponse(jsonqs, content_type="text/json")
```
- json.dumps()
  -  object형식을 JSON형식 스트림으로 직렬화한다.
-  HttpResponse()
    -  JSON으로 데이터를 Response한다.

