# segm_ut

代码说明 0724

实验具体实现：
1.	加载论文预训练好的模型，固定backbone，去训练uncertainty模块，
2.	24epoch后，将上述uncertainty 模块加载到一个scratch的模型，固定uncertainty模块，训练backbone

存在的问题：
不清楚只训练（64-24）40个epoch的话，模型是否来得及收敛

预训练模型下载
https://www.rocq.inria.fr/cluster-willow/rstrudel/segmenter/checkpoints/ade20k/seg_base_mask/checkpoint.pth


实验1
采用上述思路，在24epoch后，启用0-1gate
下面的./Base_16.pth是预训练模型的位置，需要对应修改

CUDA_VISIBLE_DEVICES=3 python -m segm.train --log-dir base_ug24_ft24_pe24  --dataset ade20k    --backbone vit_base_patch16_384 --decoder mask_transformer --ut 1 --ug 24  --ft 24 --pre_ck ./Base_16.pth --pre_epoch 24 

实验2
采用上述思路，在24epoch后，不启用0-1gate
下面的./Base_16.pth是预训练模型的位置，需要对应修改

CUDA_VISIBLE_DEVICES=3 python -m segm.train --log-dir base_ug0_ft24_pe24  --dataset ade20k   --backbone vit_base_patch16_384 --decoder mask_transformer --ut 1 --ft 24 --pre_ck ./Base_16.pth --pre_epoch 24 
