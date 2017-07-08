-- Server thing

AddCSLuaFile("client/whatever.lua")

util.AddNetworkString( "halodraw" )
hook.Add("ScalePlayerDamage", "HeadshotHalo", dmghandler)

local hdonly = true;

local function dmghandler(ply, hgroup, data)
if ( hgroup == HITGROUP_HEAD ) and ( hdonly ) then
data:ScaleDamage( 12 )
else if (hdonly) and ( data:GetAttacker():GetActiveWeapon() != "weapon_crowbar" ) then
data:ScaleDamage( 0 )
net.Start()
net.WriteEntity( data:GetAttacker() )
net.Send( ply )
end
end


-- client thing

net.Receive( "halodraw", function(ply,len)
local tohalo = { net.ReadEntity() }
halo.Add( tohalo, Color( 255, 0, 255 ), 0, 0, 1, true, true )
end )
