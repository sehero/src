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
local lib={}

function lib.same(x) return x end

function lib.any(l) return l[math.random(1,#l)] end

function lib.anys(l,n,    t) 
  t,n = {}, n or 128
  for i=1,n do t[#t+1] = lib.any(l) end
  return t
end

local function what2do(t,f)
  if not f                 then return lib.same end
  if type(f) == 'function' then return f end 
  if type(f) == 'string'   then 
    return function (z) return z[f] end  
  end
  local m = getmetable(t)
  return m and m[f] or assert(false,"bad function")
end

function lib.bchop(t,val) 
  local lo,hi=1,#t
  while lo <= hi do
    local mid =(lo+hi) // 2
    if t[mid] > val then hi= mid-1 else lo= mid+1 end
  end
  return math.min(lo,#t)  
end

function lib.map(t,f, out)
  out={}
  f = what2do(t,f)
  if t then for i,v in pairs(t) do out[i] = f(v) end  end
  return out
end

function lib.reject(t,f)
  return lib.select(t, function (z) return not f(z) end) end

function lib.select(t,f, out)
  out={}
  for _,v in pairs(t) do 
    if f(v) then out[#out+1] = v  end end
  return out
end

function lib.copy(t)  
  return type(t) ~= 'table' and t or lib.map(t,lib.copy)
end

function lib.sort(t,f)
  if type(f) == "string" then
    return lib.sort(t, function(x,y) return x[f]<y[f] end) 
  elseif not f then
    return lib.sort(t, function(x,y) return x< y end) 
  end
  table.sort(t,f)
  return t
end

function lib.rpad(s,n)
  s = tostring(s)
  return  s .. string.rep(" ",n - #s) 
end

function lib.lpad(s,n)
  s = tostring(s)
  return  string.rep(" ",n - #s) .. s
end

function lib.split(s, sep,    t,notsep)
  t, sep = {}, sep or ","
  notsep = "([^" ..sep.. "]+)"
  for y in string.gmatch(s, notsep) do t[#t+1] = y end
  return t
end

function lib.dump(t)
   if type(t) ~= 'table' then return tostring(o) end
   local s = '{ '
   for k,v in pairs(t) do
     s = s ..k..' = ' .. lib.dump(v) .. ','
   end
   return s .. '} '
end

function lib.cache(f)
  return setmetatable({}, {
    __index=function(t,k) t[k]=f(k);return t[k] end})
end

function lib.norm(x, mu, sd)
  mu = mu or 0
  sd = sd or 1
  if x < mu-4*sd then return 0 end 
  if x > mu+4*sd then return 0 end
  return (1 / 
    (sd * math.sqrt(2 * math.pi))) * 
     math.exp(-(((x - mu) * (x - mu)) / (2 * sd^2))) 
end

return lib

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
