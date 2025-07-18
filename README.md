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
<ul>
<li>сгенерить файлы в /home/;</span></li>
<li>снять снапшот;</span></li>
<li>удалить часть файлов;</span></li>
<li>восстановиться со снапшота.</span></li>
</ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">* На дисках попробовать поставить btrfs/zfs &mdash; с кэшем, снапшотами и разметить там каталог /opt.</span></li>
</ol>
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<h3 class="western"><a name="_heading=h.df570rpzx1qg"></a><span style="font-family: Roboto, serif;"><span style="font-size: small;">Используемые ОС</span></span></h3>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Хостовая ОС: Ubuntu 24.04 Desktop, гостевая система Ubuntu 24.04 Server. VirtualBox версия 7.0.16_Ubuntu r162802</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify">&nbsp;</p>
<h3 class="western"><span style="font-family: Roboto, serif;"><span style="font-size: small;">Выполнение</span></span></h3>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Используем такой стенд:</span></span></p>
<img width="634" height="293" alt="изображение" src="https://github.com/user-attachments/assets/ae6769b2-ed4d-4c92-89c5-5259d9a690e8" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">1) Уменьшить том под / до 8G.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Подготовим временный том для / раздела:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:~# pvcreate /dev/sdb</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">  Physical volume "/dev/sdb" successfully created.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:~# vgcreate vg_root /dev/sdb</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">  Volume group "vg_root" successfully created</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:~# lvcreate -n lv_root -l +100%FREE /dev/vg_root</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">  Logical volume "lv_root" created.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Создадим на нем файловую систему и смонтируем его, чтобы перенести туда данные:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">mkfs.ext4 /dev/vg_root/lv_root</span></span></p>
<img width="715" height="288" alt="изображение" src="https://github.com/user-attachments/assets/ba311de8-3e70-4c77-bd07-e3cc049f4ba0" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">mount /dev/vg_root/lv_root /mnt</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Этой командой копируем все данные с / раздела в /mnt:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">rsync -avxHAX --progress / /mnt/</span></span></p>
<img width="776" height="77" alt="изображение" src="https://github.com/user-attachments/assets/7c32164e-df8f-423c-a42a-f782be2e6631" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Проверить, что скопировалось, можно командой ls /mnt.</span></span></p>
<img width="714" height="182" alt="изображение" src="https://github.com/user-attachments/assets/0287539c-cd17-4ee0-9068-96ff2aef52b2" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">TEXT HERE</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">TEXT HERE</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">TEXT HERE</span></span></p>
