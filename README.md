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
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;"><b>Уменьшить том под / до 8G.</b></span></span></p>
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
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Затем сконфигурируем grub для того, чтобы при старте перейти в новый /.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Сымитируем текущий root, сделаем в него chroot и обновим grub:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">for i in /proc/ /sys/ /dev/ /run/ /boot/; \</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">  do mount --bind $i /mnt/$i; done</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">chroot /mnt/</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">grub-mkconfig -o /boot/grub/grub.cfg</span></span></p>
<img width="777" height="244" alt="изображение" src="https://github.com/user-attachments/assets/0077b176-528a-4fe4-89cc-5571b01602cf" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Обновим образ initrd.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">update-initramfs -u</span></span></p>
<img width="777" height="69" alt="изображение" src="https://github.com/user-attachments/assets/fe394f49-bb73-4f49-841e-4376f6f12385" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Перезагружаемся, чтобы работать с новым разделом.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Посмотрим картину с дисками после перезагрузки:</span></span></p>
<img width="648" height="319" alt="изображение" src="https://github.com/user-attachments/assets/650d448f-3a3f-4eda-98fe-3df98c426b7d" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Теперь нам нужно изменить размер старой VG и вернуть на него рут. Для этого удаляем старый LV и создаём новый:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">lvremove /dev/ubuntu-vg/ubuntu-lv</span></span></p>
<img width="807" height="119" alt="изображение" src="https://github.com/user-attachments/assets/c47ea11a-e56d-4601-98eb-259e727bd56d" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">lvcreate -n ubuntu-vg/ubuntu-lv -L 8G /dev/ubuntu-vg</span></span></p>
<p>root@training:~# lvcreate -n ubuntu-vg/ubuntu-lv -L 8G /dev/ubuntu-vg<br />WARNING: ext4 signature detected on /dev/ubuntu-vg/ubuntu-lv at offset 1080. Wipe it? [y/n]: y<br />&nbsp; Wiping ext4 signature on /dev/ubuntu-vg/ubuntu-lv.<br />&nbsp; Logical volume "ubuntu-lv" created.</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Проделываем на нем те же операции, что и в первый раз:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">mkfs.ext4 /dev/ubuntu-vg/ubuntu-lv</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">mount /dev/ubuntu-vg/ubuntu-lv /mnt</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">rsync -avxHAX --progress / /mnt/</span></span></p>
<img width="781" height="74" alt="изображение" src="https://github.com/user-attachments/assets/8f506e3a-1e46-4da7-b6d8-a891bfe16656" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Так же, как в первый раз, cконфигурируем grub.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">for i in /proc/ /sys/ /dev/ /run/ /boot/; \</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;"> do mount --bind $i /mnt/$i; done</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">chroot /mnt/</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">grub-mkconfig -o /boot/grub/grub.cfg</span></span></p>
<img width="781" height="248" alt="изображение" src="https://github.com/user-attachments/assets/1ba81247-cfcb-4581-9ed3-9df21fe5a52f" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">update-initramfs -u</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:/# update-initramfs -u</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">update-initramfs: Generating /boot/initrd.img-6.8.0-64-generic</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">W: Couldn't identify type of root file system for fsck hook</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;"><strong>Пока не перезагружаемся и не выходим из под chroot - мы можем заодно перенести /var.</strong></span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;"><b>Выделить том под /var в зеркало.</b></span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">На свободных дисках создаем зеркало:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:/# pvcreate /dev/sdc /dev/sdd</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">  Physical volume "/dev/sdc" successfully created.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">  Physical volume "/dev/sdd" successfully created.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:/# vgcreate vg_var /dev/sdc /dev/sdd</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">  Volume group "vg_var" successfully created</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:/# lvcreate -L 950M -m1 -n lv_var vg_var</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;"> Rounding up size to full physical extent 952,00 MiB</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">  Logical volume "lv_var" created.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Создаем на нем ФС и перемещаем туда /var:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:/# mkfs.ext4 /dev/vg_var/lv_var</span></span></p>
<img width="668" height="291" alt="изображение" src="https://github.com/user-attachments/assets/23f5f777-b809-4c99-9236-9d230955a5f8" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:/# mount /dev/vg_var/lv_var /mnt</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:/# cp -aR /var/* /mnt/</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">На всякий случай сохраняем содержимое старого var:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:/# mkdir /tmp/oldvar && mv /var/* /tmp/oldvar</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Монтируем новый var в каталог /var:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:/# umount /mnt</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:/# mount /dev/vg_var/lv_var /var</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Правим fstab для автоматического монтирования /var:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:/# echo "`blkid | grep var: | awk '{print $2}'` \</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;"> /var ext4 defaults 0 0" >> /etc/fstab</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">После чего можно успешно перезагружаться в новый (уменьшенный root) и удалять временную Volume Group:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:~# lvremove /dev/vg_root/lv_root</span></span></p>
<img width="799" height="125" alt="изображение" src="https://github.com/user-attachments/assets/655870d1-431e-4e99-8db9-2823b2f97e92" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:~# vgremove /dev/vg_root</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">  Volume group "vg_root" successfully removed</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:~# pvremove /dev/sdb</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">  Labels on physical volume "/dev/sdb" successfully wiped.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;"><b>Выделить том под /home</b></span></p>
<p>Выделяем том под /home по тому же принципу что делали для /var:</p>
<p>root@training:~# lvcreate -n LogVol_Home -L 2G /dev/ubuntu-vg</p>
<p>Logical volume "LogVol_Home" created.</p>
<p>root@training:~# mkfs.ext4 /dev/ubuntu-vg/LogVol_Home</p>
<p>[root@training:~# mount /dev/ubuntu-vg/LogVol_Home /mnt/</p>
<p>root@training:~# cp -aR /home/* /mnt/</p>
<p>root@training:~# rm -rf /home/*</p>
<p>root@training:~# umount /mnt</p>
<p>root@training:~# mount /dev/ubuntu-vg/LogVol_Home /home/</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Правим fstab для автоматического монтирования /home:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:~# echo "/dev/mapper/ubuntu--vg-LogVol_Home  /home ext4 defaults 0 0" >> /etc/fstab</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">После перезагрузки видим, что /home теперь отдельно:</span></span></p>
<img width="644" height="492" alt="image" src="https://github.com/user-attachments/assets/e7fe18fd-573f-4ee8-a23d-051fa0be2e19" />
<img width="681" height="120" alt="image" src="https://github.com/user-attachments/assets/615b7dc8-dc55-4088-aa6d-3cb5995436cc" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;"><b>Работа со снапшотами</b></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Генерируем файлы в /home/:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:~# touch /home/file{1..20}</span></span></p>
<img width="681" height="574" alt="image" src="https://github.com/user-attachments/assets/0a7141f2-f0fe-41a4-a0ac-60a221fdd043" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Снимаем снапшот:</span></span></p>
<p>root@training:~# lvcreate -L 100MB -s -n home_snap /dev/ubuntu-vg/LogVol_Home<br /> Logical volume "home_snap" created.<br />root@training:~#</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Удалим часть файлов:</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">root@training:~# rm -f /home/file{11..20}</span></span></p>
<img width="681" height="354" alt="image" src="https://github.com/user-attachments/assets/3a941b93-1e7d-4b03-a812-ccf4cbbca93c" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Восстановим файлы из снапшота:</span></p>
<p>root@training:~# umount /home<br />root@training:~# lvconvert --merge /dev/ubuntu-vg/home_snap<br /> Merging of volume ubuntu-vg/home_snap started.<br /> ubuntu-vg/LogVol_Home: Merged: 100,00%<br />root@training:~#</p>
<p>root@training:~# mount /dev/mapper/ubuntu--vg-LogVol_Home /home<br />root@training:~# ls -l /home</p>
<img width="681" height="575" alt="image" src="https://github.com/user-attachments/assets/eff94e25-974f-4b63-a4ce-8c1528a51f67" />
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Файлов снова 20. Файлы успешно восстановлены с помощью снапшота.</span></span></p>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Задание завершено.</span></span></p>


