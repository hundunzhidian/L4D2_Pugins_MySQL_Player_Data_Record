# L4D2_Pugins_MySQL_Player_Data_Record
注意：数据表编码必须全部为utf-8
插件效率还可以，但是还有优化的余地，下面是优化建议:
在灭图重启round_start或者在地图刚加载OnMapStart的时候，会执行
for (new i=1;i<=MaxClients;i++)
{
	if (IsClientConnected(i) && IsClientInGame(i))
	{
	
		GetClientAuthId(i,AuthId_Steam2,id,sizeof(id));
		if (!StrEqual(id, "BOT"))
			if(!Update_DATA(i,false,true)) return;
	}
}
来更新玩家数据到数据库，可以直接更改Update_DATA函数，使用循环来执行预编译(DBStatement)绑定功能，绑定一次玩家数据后立即跟新数据，
极大的提高了性能，免去了每个玩家都要进行数据库连接断开的步骤
