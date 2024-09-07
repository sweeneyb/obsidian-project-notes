## Met information

https://github.com/metmuseum/openaccess
https://www.metmuseum.org/art/the-collection
https://metmuseum.github.io/#search
https://www.metmuseum.org/art/collection/search/459001
https://www.metmuseum.org/art/collection/search/415868?showOnly=openAccess&amp;ft=*&amp;offset=40&amp;rpp=40&amp;pos=63
https://metmuseum.github.io/#search
https://diff.wikimedia.org/2017/02/07/the-met-public-art-creative-commons/

api hit for details
https://collectionapi.metmuseum.org/public/collection/v1/objects/459001

### Links to specific items to show how to move through the collection
https://collectionapi.metmuseum.org/public/collection/v1/search?q=Composition
https://collectionapi.metmuseum.org/public/collection/v1/objects/490012

## UI Components
A better swipe impl that those I've tried:
https://codesandbox.io/s/github/FormidableLabs/react-swipeable/tree/main/examples?file=/app/components.tsx



# label details
#### json details
https://collectionapi.metmuseum.org/public/collection/v1/objects/459001

curl https://collectionapi.metmuseum.org/public/collection/v1/objects/459001 | jq '{"title": .title, "artist": .artistDisplayName, "image": .primaryImageSmall}'
#### Preview
https://collectionapi.metmuseum.org/api/collection/v1/iiif/459001/preview
#### source
https://www.metmuseum.org/art/collection/search/459001