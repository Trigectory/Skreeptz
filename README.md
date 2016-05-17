-- > CREATED BY WULFBUG9 2014
-- > Seriously, I made this. (wulfbug9)
-- > I'm releasing a ton of my other scripts on this pastebin, too. So go check them out.
function rand(a)return (math.random()-.5)*2*a end
function q(f,arg)return coroutine.resume(coroutine.create(f),unpack(arg or {}))end
function fade(p,s)q(function(part,start)for i=start,1,.05 do part.Transparency = part.Transparency+0.05 wait(1/30)end end,{p,s})end
function appear(p,s)q(function(part,start)for i=start,0,-.05 do part.Transparency = part.Transparency-0.05 wait(1/30)end end,{p,s})end
function Part(Name,Parent,Size,CFrame,Color,Trans,Anch,Can,Mat,Ref)
	local p = Instance.new("Part",Parent)p.Name = Name
	p.FormFactor = "Custom"p.Size = Size
	p.Anchored = Anch p.CFrame = CFrame
	p.BrickColor = BrickColor.new(Color)p.Transparency = Trans
	p.TopSurface = 0 p.CanCollide = Can
	p.BottomSurface = 0 p.Material = Mat
	p.Reflectance = Ref or 0;p:BreakJoints()
	p.Locked = true;return p
end
function WedgePart(Name,Parent,Size,CFrame,Color,Trans,Anch,Can,Mat,Ref)
	local p = Instance.new("WedgePart",Parent)p.Name = Name
	p.FormFactor = "Custom"p.Size = Size
	p.Anchored = Anch p.CFrame = CFrame
	p.BrickColor = BrickColor.new(Color)p.Transparency = Trans
	p.TopSurface = 0 p.CanCollide = Can
	p.BottomSurface = 0 p.Material = Mat
	p.Reflectance = Ref or 0;p:BreakJoints()
	p.Locked = true;return p
end
function CornerWedgePart(Name,Parent,Size,CFrame,Color,Trans,Anch,Can,Mat,Ref)
	local p = Instance.new("CornerWedgePart",Parent)p.Name = Name;p.Size = Size
	p.Anchored = Anch p.CFrame = CFrame
	p.BrickColor = BrickColor.new(Color)p.Transparency = Trans
	p.TopSurface = 0 p.CanCollide = Can
	p.BottomSurface = 0 p.Material = Mat
	p.Reflectance = Ref or 0;p:BreakJoints()
	p.Locked = true;return p
end
function Mesh(Parent,Type,Scale,ID,TID)
	local m = Instance.new("SpecialMesh",Parent)m.MeshType = Type
	m.Scale = Scale or Vector3.new(1,1,1)
	if ID then m.MeshId = ID end if TID then m.TextureId = TID end
	return m
end
function Weld(p1,p2,c0,c1)
	local w = Instance.new("Weld",p1)w.Part0 = p1;w.Part1 = p2
	w.C0,w.C1 = c0 or CFrame.new(),c1 or CFrame.new()
	return w
end
function cslerp(c1,c2,t)
	local function s(a,b,c)return (1-c)*a+(c*b)end 
	local com1 = {c1.X,c1.Y,c1.Z,c1:toEulerAnglesXYZ()}
	local com2 = {c2.X,c2.Y,c2.Z,c2:toEulerAnglesXYZ()}
	for i,v in pairs(com1)do com1[i] = s(v,com2[i],t)end
	return CFrame.new(com1[1],com1[2],com1[3])*CFrame.Angles(select(4,unpack(com1)))
end
local char
---------------------------------------------
player = game:service("Players").LocalPlayer
repeat wait() char = player.Character until char
pcall(function()char:FindFirstChild("Animate"):Destroy()end)
root = char:WaitForChild("HumanoidRootPart")
torso = char:WaitForChild("Torso")
humanoid = char:WaitForChild("Humanoid")
mouse = player:GetMouse()
step = game:service("RunService").Stepped
asset = "http://www.roblox.com/asset/?id="
meshes = {["blast"] = 20329976,["ring"] = 3270017,["spike"] = 1033714,["cone"] = 1082802,["crown"] = 20329976,["cloud"] = 1095708,["diamond"] = 9756362}
sounds = {["explode"] = 130792180;}
colour = "White"
scolour = tostring(BrickColor.random())--"Lime green"
Attacking = false
local bv,bp,bg
hover = 10
carspeed = 60
keysdown = {}
c0ls = CFrame.new(-1,0.5,0)*CFrame.Angles(math.pi/6,0,0)
c0rs = CFrame.new(1,0.5,0)*CFrame.Angles(math.pi/6,0,0)
c1ls = CFrame.new(0.5,0.5,0)
c1rs = CFrame.new(-0.5,0.5,0)
c0tw = CFrame.new(0,0,0)
c1tw = CFrame.new(0,0,0)
rs = Weld(torso,char:WaitForChild("Right Arm"),c0rs,c1rs)
ls = Weld(torso,char:WaitForChild("Left Arm"),c0ls,c1ls)
tw = Weld(root,torso,c0tw,c1tw)
---------------------------------------------
function Smoke(origin,color)
	local p = Part("Effect",workspace,Vector3.new(2,2,2),origin*CFrame.new(rand(10),-1,rand(10)),color or "Black",.1,false,false,"SmoothPlastic")
	local m = Mesh(p,"Sphere",Vector3.new(1.25,1.25,1.25))
	local bp = Instance.new("BodyPosition",p)bp.D = 100 bp.P = 100 bp.position = p.Position+Vector3.new(0,7,0)
	q(function(pa,me)
		fade(pa,.1)
		for i=25,100 do
			me.Scale = me.Scale+Vector3.new(0.15,0.1,0.15)
			wait(1/30)
		end
		pa:Destroy()
	end,{p,m})
end
function crownExplode(origin,color,size)
	local p = Part("Effect",workspace,Vector3.new(size,size,size),origin,color,.2,true,false,"SmoothPlastic")
	local m = Mesh(p,"FileMesh",Vector3.new(size/2,size/2,size/2),asset..meshes["crown"])
	q(function(pa,me)
		for i=.2,1,.025 do
			me.Scale = me.Scale+Vector3.new(0.75,0.75,0.75)
			pa.Transparency = i
			wait(1/30)
		end
		pa:Destroy()
	end,{p,m})
end
function quickSound(id,v)
	local s = Instance.new("Sound",workspace)
	s.SoundId = id
	s.PlayOnRemove = true
	s.Volume = v or 1
	delay(0.025,function()s:remove()end)
end
function checkDmgArea(origin,dmg,d)
	for i,v in pairs(workspace:children())do
		if v~=char and v:FindFirstChild("Torso") then
			local h;
			for _,k in pairs(v:children())do if k:IsA("Humanoid") then h = k end end
			local dist = (origin.p - v:FindFirstChild("Torso").CFrame.p).magnitude
			if dist < d and h~=nil then
				h.Health = h.Health - dmg
			end
		end
	end
end
function Shoot(start,dmg)
	dmg = dmg or 15
	local vel = start.lookVector
	local p = Part("Bullet",workspace,Vector3.new(4,4,4),start,"Black",0,true,false,"SmoothPlastic")
	local m = Mesh(p,"Sphere")
	local num = 0
	local ign = char:children()
	local connect
	connect = step:connect(function()
		num = num + 1
		local pp = p.Position
		local h,po
		vel = vel - Vector3.new(0,math.min(999.5,vel.magnitude/50),0)
		repeat
			local r = Ray.new(pp,vel.unit*math.min(999.5,vel.magnitude/100+4))
			h,po = workspace:FindPartOnRayWithIgnoreList(r,ign)
			if h then
				if h.CanCollide then break
				else table.insert(ign,h)h = nil
				end
			else break
			end
		until false
		p.CFrame = CFrame.new(po,po+vel)
		q(function(b)
			local a = b:Clone()
			a.Parent = workspace
			for i=1,-.05,-.05 do
				wait()
				a:FindFirstChild("Mesh").Scale = Vector3.new(i,i,i)
				a.Transparency = a.Transparency + .05
			end
			a:Destroy()
		end,{p})
		if h or num > 300 then
			local cf = p.CFrame
			for i=1,3 do Smoke(cf*CFrame.new(0,4,0),"Black")end
			crownExplode(CFrame.new(cf.x,cf.y,cf.z),"Black",2)
			quickSound(asset..sounds["explode"],2)
			checkDmgArea(cf,dmg,10)
			p:Destroy()
			connect:disconnect()
		end
	end)
end
function Fire()
	Attacking = true
	for i=1,10 do
		wait(1/30)
		local speed = i/10
		rs.C0 = cslerp(rs.C0,c0rs*CFrame.Angles(-math.pi/2.5,0,0),speed)
		ls.C0 = cslerp(ls.C0,c0ls*CFrame.Angles(-math.pi/2.5,0,0),speed)
		tw.C0 = cslerp(tw.C0,c0tw*CFrame.Angles(0,math.pi/2,0),speed)
	end
	local b = Part("Bullet",char,Vector3.new(4,4,4),torso.CFrame,"Black",0,false,false,"SmoothPlastic")
	Mesh(b,"Sphere")
	local w = Weld(torso,b,CFrame.new(0,0,-.5))
	for i=1,5 do
		wait(1/30)
		local speed = i/5
		rs.C0 = cslerp(rs.C0,c0rs*CFrame.Angles(math.pi/2.5,0,0),speed)
		ls.C0 = cslerp(ls.C0,c0ls*CFrame.Angles(math.pi/2.5,0,0),speed)
		tw.C0 = cslerp(tw.C0,c0tw,speed)
		w.C0 = cslerp(w.C0,CFrame.new(0,5,-3),speed)
	end
	Shoot(b.CFrame,52.5)
	b:Destroy()
	for i=1,10 do
		wait(1/30)
		local speed = i/20
		rs.C0 = cslerp(rs.C0,c0rs,speed)
		ls.C0 = cslerp(ls.C0,c0ls,speed)
		tw.C0 = cslerp(tw.C0,c0tw,speed)
	end
	Attacking = false
end
---------------------------------------------
pcall(function()char:FindFirstChild("CAR"):Destroy()end)
model = Instance.new("Model",char)
model.Name = "CAR"
base = Part("P",model,Vector3.new(6,6,6),torso.CFrame*CFrame.new(6,6,0),colour,0,false,false,"Plastic")
torso.CFrame = base.CFrame
basem = Mesh(base,"Sphere",Vector3.new(1,.5,1))
side = Part("P",model,Vector3.new(6,2,6),torso.CFrame*CFrame.new(6,6,0),colour,0,false,false,"Plastic")
sidem = Mesh(side,"FileMesh",Vector3.new(5.375,5.375,20),asset..meshes["ring"])
sidew = Weld(base,side,CFrame.new(0,1,0)*CFrame.Angles(math.pi/2,0,0))
side2 = Part("P",model,Vector3.new(6,2,6),torso.CFrame*CFrame.new(6,6,0),scolour,0,false,false,"Plastic")
side2m = Mesh(side2,"FileMesh",Vector3.new(5.375,5.375,5),asset..meshes["ring"])
side2w = Weld(side,side2,CFrame.new(0,0,-1.25))
under = Part("P",model,Vector3.new(2,6,2),torso.CFrame*CFrame.new(6,6,0),scolour,0,false,false,"Plastic")
underm = Mesh(under,"FileMesh",Vector3.new(2,6,2),asset..meshes["spike"])
underw = Weld(base,under,CFrame.new(0,-1,0)*CFrame.Angles(math.pi,0,0))
prop = Part("P",model,Vector3.new(0.5,0,4),torso.CFrame*CFrame.new(6,6,0),scolour,0,false,false,"Plastic")
propm = Mesh(prop,"Sphere")
propw = Weld(under,prop,CFrame.new(0,2.25,0))
torweld = Weld(base,root,CFrame.new(0,3,-1))
bp = Instance.new("BodyPosition")
bp.maxForce = Vector3.new(0,1/0,0)
bv = Instance.new("BodyVelocity")
bv.maxForce = Vector3.new(1/0,0,1/0)
bg = Instance.new("BodyGyro")
bg.maxTorque = Vector3.new(1/0,1/0,1/0)
humanoid.WalkSpeed = 0
---------------------------------------------
mouse.KeyDown:connect(function(key)
	key:lower()
	keysdown[key] = true
end)
mouse.KeyUp:connect(function(key)
	key:lower()
	keysdown[key] = false
end)
mouse.Button1Down:connect(function()
	if not Attacking then
		Fire()
	end
end)
local function bn(key) return keysdown[key]and 1 or 0 end
---------------------------------------------
step:connect(function()
	hover = hover-bn("e")+bn("q")
	bp.Parent,bg.Parent,bv.Parent = base,base,base
	bp.position = Vector3.new(0,math.sin(tick())+hover,0)
	bg.cframe = workspace.CurrentCamera.CoordinateFrame
	local vel = workspace.CurrentCamera.CoordinateFrame:vectorToWorldSpace(Vector3.new(0-bn("a")+bn("d"),0,0-bn("w")+bn("s")))*Vector3.new(1,0,1)
	bv.velocity = (vel.magnitude > 0 and vel.unit*carspeed)or Vector3.new(0,0,0)
	propw.C0 = propw.C0*CFrame.Angles(0,.2,0)
end)
