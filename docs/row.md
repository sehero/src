<a class=sehero name=top> 
<img align=right width=280 src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/2c218305-10f7-4dc5-b98c-8944ea7c6b98/d92z77z-85f30213-a950-43e6-93aa-ca906c6b4aac.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOiIsImlzcyI6InVybjphcHA6Iiwib2JqIjpbW3sicGF0aCI6IlwvZlwvMmMyMTgzMDUtMTBmNy00ZGM1LWI5OGMtODk0NGVhN2M2Yjk4XC9kOTJ6Nzd6LTg1ZjMwMjEzLWE5NTAtNDNlNi05M2FhLWNhOTA2YzZiNGFhYy5qcGcifV1dLCJhdWQiOlsidXJuOnNlcnZpY2U6ZmlsZS5kb3dubG9hZCJdfQ.BY_xZ9vtOug8jM-lzpvybhtGb2rItxHbWs1sDGlNEAY">
<h1><a href="/README.md#top">SE for super heroes: an AI approach</a></h1> 
<p> <a
href="https://github.com/sehero/lua/blob/master/LICENSE">license</a> :: <a
href="https://github.com/sehero/lua/blob/master/INSTALL.md#top">install</a> :: <a
href="https://github.com/sehero/lua/blob/master/CODE_OF_CONDUCT.md#top">contribute</a> :: <a
href="https://github.com/sehero/lua/issues">issues</a> :: <a
href="https://github.com/sehero/lua/blob/master/CITATION.md#top">cite</a> :: <a
href="https://github.com/sehero/lua/blob/master/CONTACT.md#top">contact</a> </p><p> 
<img src="https://img.shields.io/badge/license-mit-red">   
<img src="https://img.shields.io/badge/language-lua-orange">    
<img src="https://img.shields.io/badge/purpose-ai,se-blueviolet"><br>
<a href="https://zenodo.org/badge/latestdoi/263210595"><img src="https://zenodo.org/badge/263210595.svg" alt="DOI"></a><br>
<img src="https://img.shields.io/badge/platform-mac,*nux-informational"><br>
<a href="https://travis-ci.org/github/sehero/lua"><img 
src="https://travis-ci.org/sehero/lua.svg?branch=master"></a><br>  
</p>
local the = require "the"
local Row = the.class()

function Row:_init(cells) 
  self.dom = 0
  self.best = false
  self.cells = cells 
end
  
function Row:dist(other,cols,p,    n,d,x,y,d0) 
  p=p or the.dist.p
  n,d = 0,0
  for pos,col in pairs(cols) do
    n = n +1
    x = self.cells[pos]
    y = other.cells[pos]
    d0= col:dist(x,y)
    d = d + d0^p
  end
  return (d / n) ^ (1/p)
end

function Row:dominates(other,cols,    s1,s2,n,x,y,x1,y1)
   s1,s2,n =  0,0,#cols
   for pos,col in pairs(cols) do
     x  = self.cells[pos]
     y  = other.cells[pos]
     x1 = col:norm(x)
     y1 = col:norm(y)
     s1 = s1 - 10^(col.w*(x1-y1)/n)
     s2 = s2 - 10^(col.w*(y1-x1)/n)
   end
  return s1/n < s2/n 
end

return Row

## Copyright

(c) 2020, Tim Menzies

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
