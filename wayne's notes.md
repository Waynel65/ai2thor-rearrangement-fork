#### command to run pre-trained model

allenact baseline_configs/one_phase/one_phase_rgb_clipresnet50_dagger.py \
-c pretrained_model_ckpts/exp_OnePhaseRGBClipResNet50Dagger_40proc__stage_00__steps_000065083050.pt \
--extra_tag $CURRENT_TIME \
--eval