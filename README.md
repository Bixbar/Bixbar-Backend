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
### redirectBixbar(request):
- HttpResponseRedirect()
  ```python
  return HttpResponseRedirect('https://www.notion.so/ccookncook/Bixbar-b5401104a0d64fdc838d27505fbf27b2')
  ```
  - url이 www.Bixbar.com 이면 Notion Page로 Redirect해준다.

### lookupQuery(queryType, q):

- Papago API
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
