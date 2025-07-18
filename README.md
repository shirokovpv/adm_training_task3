# adm_training_task3
<h1 align="center">Занятие 3. Файловые системы и LVM</h1>
<h3 class="western"><a name="_heading=h.h6i87lkp3f19"></a> <span style="font-family: Roboto, serif;"><span style="font-size: small;">Описание домашнего задания</span></span></h3>
<p>&nbsp;</p>
<p><span style="font-weight: 400;">На виртуальной машине с Ubuntu 24.04 и LVM.</span></p>
<ol>
<li style="font-weight: 400;"><span style="font-weight: 400;">Уменьшить том под / до 8G.</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Выделить том под /home.</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Выделить том под /var - сделать в mirror.</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">/home - сделать том для снапшотов.</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Прописать монтирование в fstab. Попробовать с разными опциями и разными файловыми системами (на выбор).</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">Работа со снапшотами:</span></li>
<ol>
<li style="font-weight: 400;"><span style="font-weight: 400;">сгенерить файлы в /home/;</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">снять снапшот;</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">удалить часть файлов;</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">восстановиться со снапшота.</span></li>
</ol>
<li style="font-weight: 400;"><span style="font-weight: 400;">* На дисках попробовать поставить btrfs/zfs &mdash; с кэшем, снапшотами и разметить там каталог /opt.</span></li>
</ol>
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<h3 class="western"><a name="_heading=h.df570rpzx1qg"></a><span style="font-family: Roboto, serif;"><span style="font-size: small;">Используемые ОС</span></span></h3>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Хостовая ОС: Ubuntu 24.04 Desktop, гостевая система Ubuntu 24.04 Server. VirtualBox версия 7.0.16_Ubuntu r162802</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify">&nbsp;</p>
<h3 class="western"><span style="font-family: Roboto, serif;"><span style="font-size: small;">Выполнение</span></span></h3>
